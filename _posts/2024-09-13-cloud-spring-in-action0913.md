---
title: 클라우드 네이티브 인 액션(11) - 스프링 시큐리티, Oauth2
author: minseok
date: 2024-09-13 00:02:00 +0800
categories: [Book, Cloud-Native-Spring-In-Action]
tags: [Spring]
math: true
mermaid: true
image: 
    path: /assets/img/cloudNativeSpringInAction/main.png
    alt: Cloud Native Spring In Action
comments: true
---

> 클라우드 네이티브 스프링 인 액션 서적의 데모 프로젝트를 모방하였습니다.
[깃 레포지토리](https://github.com/kkminseok/spring-cloud-native-example)
{: .prompt-info}


기존 프로젝트에는 문제가 있다. 앞단에서 요청을 받는 edge-service가 사용자 요청에 대한 인증을 처리하고 각 다른 서비스들에게 인증컨택스트를 전달해야하는데 그런 기능이 없다. 이는 OAuth2와 액세스 토큰을 통해서 해결한다.
또한 사용자 권한에 따라 스프링 부트에서 노출되는 엔드포인트도 보호할 것이다.

현재 사용하고 있는 키클록은 사용자 인증을 진행할 때 다른 애플리케이션에 접근할 수 있는 **엑세스 토큰**을 발행하는데, 이를 가지고 다른 서비스들에 접근할 것이다. Oauth2자체가 사용자 대신해서 다른 애플리케이션이 보호하고 제한된 리소스에 접근할 수 있게 해주는 권한 프레임워크이기에 이를 통해서 다른 서비스에서 데이터를 가져올 것이다.

OIDC 인증 흐름에서 구현된 역할은 다음과 같이 나눌 수 있다.

- 인증 서버: 사용자 인증, 토큰 발행, 갱신 등을 담당하는 개체. 현재 프로젝트에서는 keycloak이다
- 사용자: 애플리케이션에 인증된 액세스를 얻기 위해 인증 서버에 로그인하는 사람
- 클라이언트: 사용자에게 인증을 요청하고 사용자를 대신해 보호된 리소스에 액세스할 수 있는 권한을 요청하는 애플리케이션, 즉 다른 애플리케이션에 필요한 정보를 가져오기 위해 요청보낼 주체가 된다.
- 리소스 서버: 클라이언트가 요청을 보내면 이를 응답해주는 서버다. 

스프링 클라우드 게이트웨이를 수정해서 보호된 자원에 액세스해볼 것이다.

전체적인 흐름을 다시 생각해자.

1. 키를록에서 인증을 성공하면 ID토큰, 액세스 토큰 발행
2. ID토큰은 시용자 정보 추출 및 사용자 세션에 대한 콘택스트를 구성하는데 사용하고 액세스 토큰은 다른 애플리케이션에 요청을 보낼때 사용된다.
3. 다른 애플리케이션에 요청을 보낼때 HTTP 헤더에 토큰값을 포함하여 요청을 보내게 된다.

이를 **토큰 릴레이**라고 하며 굳이 외울필요는 없을 것 같다. 스프링 클라우드 게이트웨이에서 내장 필터로 위의 과정을 쉽게 진핼할 수 있게끔 도와준다. 필터가 활성화되면 다른 서비스로 요청을 보낼때 액세스 토큰이 자동 포함되기 때문이다.

## 스프링 클라우드 게이트웨이 토큰 릴레이 패턴 적용

스프링 클라우드 게이트웨이를 사용하고 있다면 해당 패턴을 적용하기 너무 쉽다.

`yml`파일에 추가만 해주면 된다.

```yml
spring:
  cloud:
    gateway:
      default-filters:
      - TokenRelay
      - ...
```

이렇게 하면 다른 서비스로 요청을 보낼때 마다 `Authorization: Bearer <access_token>` 값을 포함해서 보내게 된다.

기본적으로 스프링 시큐리트는 액세스 토큰을 메모리에 저장하는데 이중화 구조에서는 동기화 문제가 발생할 수 있다. 따라서 레디스에 따로 저장해야한다.

기본적으로 스프링 시큐리티는 액세스 토큰을 `Oauth2AuthorizedClient`에 저장하는데 이는 `ServerOAuth2AuthorizedClientRepository`를 통해서 접근이 가능하다. 레디스에 저장하기 위해서는 `ServerOAuth2AuthorizedClientRepository`구현체인 `WebSessionServerOAuth2AuthorizedClientRepository`를 통해서 웹 세션에 데이터를 저장하여 스프링 세션을 통해 자동으로 레디스에 저장하게한다.

따라서 `ServerOAuth2AuthorizedClientRepository` 유형의 빈을 정의해서 확인해본다.

```java
//SecurityConfig.java
@Bean
ServerOAuth2AuthorizedClientRepository auth2AuthorizedClientRepository(){
    return new WebSessionServerOAuth2AuthorizedClientRepository();
}
```

ID토큰과 액세스 토큰 모두 사용자에 대한 정의를 가질 수 있고 보통 액세스 토큰이는 JWT기술이 쓰이는데 JWT는 클레임 형식으로 지정된다. 클레임 형식은 JSON형식의 키-값으로 다들 한 번씩 구성해봤을 것이다. 이러한 클레임에 대한 액세스는 **범위**를 통해서 제어된다. 

범위란 간단하다. 소셜로그인을 하면 어떤 정보를 취득할 것인지에 대해 동의를 물어보는 창이 나오는데 
이러한 동의 기능은 범위를 기반으로 동작하게 된다. 즉, 어떤 클라이언트가 보호된 기능에 접근할 수 있는 범위를 지정한다.

인증된 사용자에게 할당된 역할을 목록으로 roles 클레임을 설정하는 방법을 볼 것이다. 그 후 roles라는 범위를 사용해 해당 클레임에 대한 액세스 권한을 edge-service에 부여하고 Keycloak에게 ID토큰과 액세스 토큰에 인증된 사용자에게 할당된 역할을 포함하도록 지시할 것이다.

이러헥 보면 이해가 안되는데.. 진행하다보면 이해가 될 것이다.

## 토큰 사용자 지정 및 사용자 역할 전파

### 1. 키클록에서 사용자 역할에 대한 액세스 설정

키클록은 roles 클레임에 포함된 사용자의 역할에 대해 애플리케이션이 액세스할 수 있도록 **roles**라는 범위가 사전에 설정되어 있다. 책에서는 이러한 방식이 불편하다고 바꾼다고 한다. 잘 이해가 되지 않지만 바꿔보도록 한다.

keycloak console에서 admin계정으로 접속한 뒤, PolarBookshop relam에 접속한다. 이후 Client scopes 메뉴에 들어가면 **roles**라는 설정값이 있는데 이를 눌러줘서 **User Realm Role**에 접근하여 속성을 바꿔준다.

![](/assets/img/cloudNativeSpringInAction/12_keycloak_console.png)


![](/assets/img/cloudNativeSpringInAction/12_keycloak-role.png)

### 2. 스프링 시큐리티 사용자 역할에 대한 액세스 설정

이제 ID토큰과 액세스 토큰의 roles 클레임을 통해 인증된 사용자 역할을 제공하도록 설정하였기에 서비스에서 클라이언트 등록할 때 roles 범위를 포함하도록 업데이트 하도록한다.

edge-service에서 yml파일을 변경해준다.

```yml
spring:
  security: 
    oauth2:
      client:
        registration:
          keycloak:
            client-id: edge-service
            client-secret: polar-keycloak-secret
            scope: openid,roles # 추가
        provider:
          keycloak:
            issuer-uri: http://localhost:8080/realms/PolarBookshop

```

### 3. ID토큰에서 사용자 정보 추출

앞에서 사용자 역할 목록을 하드코딩했는데, ID토큰값을 가지고 있지 않았기 때문이다.

이제 새로운 roles 클레임을 포함해 ID 토큰의 모든 클레임에 액세스 할 수 있다.

```java
@GetMapping("user")
public Mono<User> getUser(@AuthenticationPrincipal OidcUser oidcUser) {
        var user = new User(
                oidcUser.getPreferredUsername(),
                oidcUser.getGivenName(),
                oidcUser.getFamilyName(),
                oidcUser.getClaimAsStringList("roles")
        );
        return Mono.just(user);
}
```

다음은 리액티브로 구성되지 않은 카탈로그 서비스에서 액세스 토큰을 어떻게 처리할지 살펴본다.

## 스프링 시큐리티 및 OAuth2를 통한 API 보호(non reactive)

카탈로그 서비스는 데이터를 전달해줄 리소스 서버가 될 것이므로 관련 의존성을 추가해준다.

```gradle
implementation 'org.springframework.boot:spring-boot-starter-oauth2-resource-server'
```

다음은 스프링 시큐리티와 키클록 통합을 진행한다.

스프링 시큐리티는 엔드포인트를 보호하기 위해 jwt와 opaque라는 토큰 2가지 유형의 액세스 토큰을 사용한다. 보통 Jwt를 사용하고 이 토큰을 통해서 정보를 줄 수 있다. opaque라는 토큰을 사용하면 토큰과 관련된 정보를 매 번 키클록에 요청해야하는 불편함이 있다.

해당 리소스 서버에서 키클록과 통합하기 위해서는 `peorperties`만 수정해주면 된다.
JWT를 사용하면 애플리케이션은 토큰 서명을 확인하는데 필요한 공개키를 가져오기 위해 keycloak에 연결한다. 단지 이 공개키를 가져와야할 url만 설정해주면된다.

요청을 수신했을때 Authorization 헤더에 액세스 토큰이 있으면 스프링 시큐리티는 키클록이 제공하는 공개키를 사용해 토큰의 서명을 확인하고 `JwtDecoder`를 통해 클레임을 찾아낸다. 이는 내부적으로 구현되어 있기에 개발자는 신경쓰지 않아도 된다.

```properties
spring.security.oauth2.resourceserver.jwt.issuer-uri=http://localhost:8080/realms/PolarBookshop
```

이제 통합이 완료되었고, 엔드포인트 보호를 위한 몇 가지 기본 보안 정책을 정의해야한다.

- GET 요청은 인증 없이 허용되어야 한다.
- 그 외 모든 요청은 인증이 필요하다
- JWT 인증을 사용해야한다.
- JWT 인증을 처리하기 위한 흐름은 상태를 가지지 않아야 한다.

마지막은 무슨말이냐면 지금 제공되는 애플리케이션은 Authorzation헤더에서 JWT 토큰값만 가지고 인증을 진행하게 되는데 이 값을 따로 저장하지 않은 무상태를 유지해야된다는 뜻이다.

이제 카탈로그 서비스에 대해 스프링 시큐리티 설정을 해준다.

```java
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers(HttpMethod.GET, "/", "/books/**").permitAll()// 루트와 책 정보는 인증없이 접근가능
                        .anyRequest().hasRole("employee") //나머지는 employee 역할을 가지고 있어야함.
                )
                .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))// JWT인증을 하도록 지정
                .sessionManagement(sessionManagement ->
                        sessionManagement.sessionCreationPolicy(SessionCreationPolicy.STATELESS)) // 각 요청에 대해 세션 생성을 하지않아 무상태를 유지하겠다는 뜻 
                .csrf(csrf -> csrf.disable()) // 상태를 갖지 않기에 브라우저 기반 클라이언트가 관여되지 않음. 따라서 csrf보호는 비활성화해도됨.
                .build();
    }
}
```

> 이 코드는 실제 예제와 다르다. 스프링 시큐리티 최신버전에서는 람다방식으로 설정을 구성하게끔 하였기에 내가 람다식으로 바꿔서 작성하였다.
{: .prompt-warning}




