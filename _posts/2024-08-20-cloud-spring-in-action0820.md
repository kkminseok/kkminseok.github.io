---
title: 클라우드 네이티브 인 액션(11) - 보안, SPA, 키클록, OpenID, Connect
author: minseok
date: 2024-08-20 00:02:00 +0800
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


보안은 눈에 보이기에는 힘들지만 중요도가 굉장히 높은 부분이다.

나는 회사 일을 하면서 보안에 대해서 생각하기도 한다.. 초기 프로젝트 설계할때 보안에 대해 고려할 수 밖에 없으며, 여러 페이지가 연동되어있을때 보안및 인가를 어떻게 처리할지도 고민이 되는 부분이 많았다.

액세스 제어를 할 때에는 크게 3가지 단계를 따라야한다.

1. 식별: 유저명이나 이메일 같이 본인을 식별할 수 있는 것을 확인
2. 인증: 식별에서 제공한 정보를 토큰, 인증서 등을 통해 검증하는 절차
3. 인가: 인증 후에 사용자가 주어진 상황에서 무엇을 할 수 있는지 확인하는 절차


## Spring-Security 적용

앞에서 작성한 서비스 중 edge-service가 모든 요청의 진입점이므로 해당 서비스에 관련 의존성을 추가해준다


```gradle
implementation 'org.springframework.boot:spring-boot-starter-security'
```

보안과 관련된 모든 설정은 한군데서 관리하기 위해 `SecurityConfig` 클래스를 생성하여 빈을 추가한다.

```java
//웹 플럭스 제공
@EnableWebFluxSecurity
public class SecurityConfig {

    @Bean
    SecurityWebFilterChain securityFilterChain(ServerHttpSecurity http) {
        return http
                .authorizeExchange(exchange -> exchange.anyExchange().authenticated()) // 모든 요청에 대해 인증이 이뤄져야함.
                .formLogin(Customizer.withDefaults()) //시큐리티가 제공하는 로그인 방식을 통해 인증을 진행
                .build();
    }
}
```

프레임워크가 제공하는 로그인 페이지를 사용하며, 인증이 되지 않으면 해당 로그인 페이지로 자동 리다이렉션 된다.

로그인 정보중 패스워드는 서버가 시작될 때 얻은 패스워드를 넣으면 된다.

```sh
Using generated security password: 89a82550-d73d-4fd4-8ea6-91218fcf0ec0
```

username: user  
password: 89a82550-d73d-4fd4-8ea6-91218fcf0ec0

이후 localhost:9000/books로 들어가면 빈 페이지가 뜬다.
9000포트에 떠있어야할 서버가 떠있지 않지만 앞서 작성한 폴백으로 인하여 빈페이지만 뜬다.

HTTP는 상태를 갖지 않기에 쿠키게 사용자 세션을 저장하여 사용하게 된다.


이러한 방식은 클라우드 네이티브에 적용하기엔 문제점이 있는데 문제점을 분석하여 이를 해결해볼 것이다.

## 키클록을 사용하여 사용자 계정 관리

위의 방식은 사용자 인증 정보가 메모리에 올라가 있고, 이를 바탕으로 로그인을 시도하고 적용했다. 
이는 프러덕션 환경에서 사용할 수 없다.

사용자 계정을 영구히 저장할 장소와 새로운 사용자를 등록할 수 있는 방안이 필요한데, 강력한 암호화 알고리즘을 통해 암호를 저장하고 데이터베이스에 무단 엑세스 하는 것도 방지해야한다.
**키클록**은 아이디 및 엑세스 관리 솔루션이다. 단일 로그인(SSO), 소셜 로그인, 사용자 연합, 다중 요소 인증(Multi-factor authentication), 등 중앙 집중식 사용자 관리를 포함해 여러 기능을 제공한다.

OAuth2, 오픈ID 커넥트, SAML 2.0과 같은 표준을 따른다.

키클록은 지속성을 위해 관계형 데이터베이스를 필요로하며 독립적으로 부트가 가능하다.

이를 확인하기 위해 도커 컴포즈에서 키클록을 정의해본다.

```sh
services:
  polar-keycloak:
    image: quay.io/keycloak/keycloak:19.0
    container_name: "polar-keycloak"
    command: start-dev # 개발 모드로 실행(내장 데이터베이스 사용)
    environment: # 어드민 유저 정보를 환경변수로 지정
      - KEYCLOAK_ADMIN=user
      - KEYCLOAK_ADMIN_PASSWORD=password
    ports:
      - 8080:8080
```

키클록은 보안영역(Security realm)을 정의해야한다.

### 키클록 보안영역 정의

키클록은 보안에 관한 모든 사항을 영역(realm)의 맥락에서 정의하는데 기본적으로 master라는 영역으로 사전 설정되어 있지만 구축되는 애플리케이션에 대한 영역을 별도로 만드는 것이 낫기에 **PolarBookShop**이라는 영역을 만들어 보안 사항을 관장한다

```sh
# keycloak 컨테이너 진입
> docker exec -it polar-keycloak bash
```

> 실행된 것을 확인하였으면 http://localhost:8080/에서 GUI를 통해 관리도 가능하다.
{: .prompt-info}

![](/assets/img/cloudNativeSpringInAction/11_keycloak.png)


키클록 어드민 CLI스크립트가 있는 곳으로 이동해야한다.

```sh
cd /opt/keycloak/bin/
```

어드민CLI를 사용하려면 키클록 컨테이너를 생성할 때 정의한 사용자 이름과 패스워드를 제공한다.

```sh
> ./kcadm.sh config credentials \
--server http://localhost:8080 \
--realm master \
--user user \
--password password

Logging into http://localhost:8080 as user user of realm master
```

이제 보안영역을 만들어본다.

```sh
# enabled = 생성된 realm 활성화 시킴
# -s : 속성 지정
> ./kcadm.sh create realms -s realm=PolarBookshop -s enabled=true

Created new realm with id 'PolarBookshop'
```

### 사용자 역할 관리

예제 애플리케이션의 사용자에는 2가지 유형이 있다.

- 고객: 책을 검색하고 구매할 수 있다.
- 직원: 카탈로그에 책을 추가하거나 기존 책을 삭제, 수정할 수 있다.

이 역할을 기반으로 애플리케이션 앤드포인트를 보호할 수 있는데 이를 **역할 기반 접근 제어(Role-Based Access Control)**이라고 한다.

```sh
# 역할 추가
> ./kcadm.sh create roles -r PolarBookshop -s name=employee
Created new role with id 'employee'
> ./kcadm.sh create roles -r PolarBookshop -s name=customer
Created new role with id 'customer'
```

이후 2명의 사용자를 만든다. **minseok Kang**은 서점의 직원이자 고객이다.

```sh
> ./kcadm.sh create users -r PolarBookshop \
-s username=minseok \
-s firstName=Minseok \
-s lastName=Kang \
-s enabled=true

Created new user with id '350d6ad7-bc75-41e6-a29a-50b0a81efb74'

> ./kcadm.sh add-roles -r PolarBookshop --uusername minseok --rolename employee --rolename customer
```

똑같은 방식으로 고객의 역할을 가진 **Bjorn Vinterberg**라는 이름을 가진 유저를 만든다.

```sh
> ./kcadm.sh create users -r PolarBookshop \
-s username=bjorn \
-s firstName=Bjorn \
-s lastName=Vinterberg \
-s enabled=true

Created new user with id '9dff5d03-4870-4606-9862-dac5fef2ee26'

> ./kcadm.sh add-roles -r PolarBookshop --uusername bjorn --rolename customer
```

실제 환경에서는 패스워드는 사용자가 선택하고 2단계 인증을 하겠지만 위 둘은 테스트 사용자이므로 임의로 패스워드를 지정해준다.

```sh
> ./kcadm.sh set-password -r PolarBookshop \
--username minseok --new-password password

> ./kcadm.sh set-password -r PolarBookshop \
--username bjorn --new-password password
```

테스트 사용자를 만들었고 애플리케이션 레벨에서 인증전략을 어떻게 사용할 수 있을지 고민해야한다.

## 오픈ID 커넥트, JWT 및 키클록을 통한 인증

현재 키클록 브라우저에서 로그인을 통해 인증을 수행하게 되는데, 애플리케이션 레벨에서 인증은 키클록에 위임하고 신원 확인만을 위한 프로토콜을 수립, 데이터 형식을 정의해야한다. 이를 정의하기 위해서는 오픈ID 커넥트, JSON 웹토큰을 통해서 진행한다.

- 오픈ID커넥트: 인증서버에게 인증을 맡기고 그 결과를 기반으로 **사용자의 신원을 확인하고 사용자의 정보를 검색할 수 있게 하는 프로토콜**이다. 인증 서버는 인증 단계의 결과를 ID토큰을 통해 클라이언트 애플리케이션에 전달한다. 
    - OAuth2는 인증 토큰을 통해 액세스 위임 문제를 해결하지만 인증 자체를 다루지는 않음. 
즉, Oauth2 + 인증 = OpenID Connect라고 이해하면 된다.
    - ID토큰이란 사용자 인증 이벤트에 대한 정보를 나타내는 JSON웹 토큰같은것들을 말한다.

OAuth2 프레임워크는 3가지 주요 행위가 있다.

1. 인증 서버: 사용자 인증 및 토큰 발행을 담당하는 개체. ex) keycloak
2. 사용자: 클라이언트 애플리케이션에 대한 인증된 액세스를 얻기 위해 인증 서버에 로그인 하는 사람들. ex) 고객
3. 클라이언트: 사용자를 인증해야하는 애플리케이션 

여담으로 키클록의 장점으로는 사용자가 로그인하기 위해 키클록과 직접 상호작용하기에 키클록 외에 어떤 시스템의 구성요소도 사용자의 정보가 노출되지 않는다는 장점이 있다.


전체적인 흐름은 다음과 같다.

1. 사용자가 애플리케이션에 요청
2. 애플리케이션이 키클록으로 302 리다이렉트(로그인 필요)
3. 사용자가 로그인 진행(요청은 애플리케이션으로)
4. 애플리케이션에서 키클록으로 요청 전달
5. 키클록이 크리덴셜 확인 후 인증코드 반환 + 애플리케이션으로 리다이렉트
6. 애플리케이션이 받은 인증코드로 키클록에 ID Token(JWT) 교환 요청
7. 키클록이 JWT반환
8. 애플리케이션이 해당 정보를 가지고 JWT파싱 및 세션 저장
9. 사용자 요청때마다 해당 정보를 사용

으로 진행된다.

이 흐름을 따라가기 위해 먼저 키클록에서 애플리케이션을 등록해본다.

### 키를록, 애플리케이션 연동

먼저 키클록 컨테이너 들어가서 세션 로그인을 진행해준다.

```sh
bash-4.4$ cd /opt/keycloak/bin/
bash-4.4$ ./kcadm.sh config credentials --server http://localhost:8080 \
> --realm master --user user --password password
```

이후 애플리케이션에 대한 클라이언트를 생성해준다.

```sh
bash-4.4$ ./kcadm.sh create clients -r PolarBookshop \ #realm
> -s clientId=edge-service \ 
> -s enabled=true \
> -s publicClient=false \ # 기밀 클라이언트
> -s secret=polar-keycloak-secret \ # 기밀클라이언트이므로 인증을 위한 시크릿 지정
> -s 'redirectUris=["http://localhost:9000","http://localhost:9000/login/oauth2/code/*"]' # 사용자 로그인, 로그아웃 후 요청을 리다이렉션할 수 있는 권한을 키클록에 부여한 애플리케이션 URL
Created new client with id 'f0e23430-3acc-4e3a-9b19-62650d011166'
```

참고로 localhost:9000/login/oauth2/code/*는 스프링 시큐리트가 기본적으로 제공하는 기본 형식이다. 로그아웃 후  리다이렉션을 지원하라면 localhost:9000도 추가해야한다.

이제 OIDC 프로토콜을 이용해서 사용자 인증을 키클록에 위임하도록 애플리케이션을 리팩터링 해야한다.

목적은 3가지다

1. 오픈ID 커넥트를 사용한 사용자 인증
2. 사용자 로그아웃 설정
3. 인증된 사용자 정보 추출

## 오픈ID 커넥트, JWT 등에 대한 애플리케이션 리팩터링


먼저 Oauth2/OIDC클라이언트 기능을 지원하는 의존성을 추가한다.

### 의존성 및 설정 수정

```gradle
implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
// test용
testImplementation 'org.springframework.security:spring-security-test'
```

스프링 시큐리티와 키클록 통합 설정을 위해서 프로젝트의 `application.yml`파일을 열어서 정보를 수정해준다. 

위에서 키클록 admin-cli에서 클라이언트를 생성할 때 식별자(edge-service)와 공유 시크릿(polar-keycloak-secret)을 정의하였다. 이를 이용해서 매핑해줘야한다.

```yml
spring:
  security:
    oauth2:
      client:
        registration:
          # 밑의 식별자 이름은 아무거나 괜ㅊ낳다.
          keycloak:
            # 키클록에 정의된 클라이언트 식별자 
            client-id: edge-service
            # 키클록에 정의된 공유 시크릿
            client-secret: polar-keycloak-secret
            # 접근권한을 갖기를 원하는 영역의 목록. openid영역은 OAuth2에서 OIDC인증 수행
            scope: openid
        provider:
          # 위의 registration에 정의된 이름과 같으면 됨.
          keycloak:
            # 특정 영역에 대한 OAuth2와 OIDC관련 모든 엔드포인트의 정보를 제공하는 키클록 URL
            issuer-uri: http://localhost:8080/realms/PolarBookshop
```

### 스프링 시큐리티 수정

기존 코드는 요청한 모든 사용자에 대해서 인증절차를 진행하도록 되어있는데 이를 바꿔서 **OIDC인증**을 사용하도록 바꿔본다.

```java
package com.polarbookshop.edgeservice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.reactive.EnableWebFluxSecurity;
import org.springframework.security.config.web.server.ServerHttpSecurity;
import org.springframework.security.web.server.SecurityWebFilterChain;

@EnableWebFluxSecurity
public class SecurityConfig {

    @Bean
    SecurityWebFilterChain securityFilterChain(ServerHttpSecurity http) {
        return http
                .authorizeExchange(exchange -> exchange.anyExchange().authenticated()) // 모든 요청에 대해 인증이 이뤄져야함.
                .oauth2Login(Customizer.withDefaults()) //Oauth2/오픈ID커넥트 방식을 이용한 로그인 인증 진행
                .build();
    }
}
```

이후 localhost:9000에 접속하면 keycloak 페이지로 자동 리다이렉트 될 것이다.

![](/assets/img/cloudNativeSpringInAction/11_keycloak_login.png)


> 이후 위에서 정의한 minseok, password 계정정보로 로그인하면 되어야하는데.. 사용자 정보를 도중에 가이드대로 바꿔주어서 isabelle 이라는 이름으로 로그인해야한다. 패스워드는 같다.
{: .prompt-warn}


로그인을 진행하고 성공하면 localhost:9000으로 리다이렉션되는데 아무런 설정도 해주지 않았기에 빈 화면만 보이게 된다.

인증 자체는 성공하였으니, 스프링 시큐리티와 인증성공한 세션에 대하여 브라우저와 통신하고 저장하게 된다.

이를 구현해보도록 한다.

## 스프링 시큐리티 컨텍스트에 인증정보 저장

사용자에 대한 정보를 매핑하기 위한 스프링 시큐리티가 제공하는 컨텍스트를 정의하고 절차를 진행해보도록 한다.

먼저 사용자에 대한 정보를 저장할 모델이 필요하므로 레코드를 하나 작성한다.

```java
package com.polarbookshop.edgeservice.user;

import java.util.List;

public record User(
        String username,
        String firstName,
        String lastName,
        List<String> roles
) { }
```

스프링 시큐리티는 인증된 사용자(Principal)에 대한 정보를 Authentication을 구현한 객체의 한 필드에 넣게 되어있다

그래서 로그인한 사용자의 인증 객체에 엑세스하는 한 가지 방법은 ReactiveSecurityContextHolder 또는 SecurityContextHolder로부터 검색해서 가져온 SecurityContext에서 해당 객체를 추출하는 것이다.




