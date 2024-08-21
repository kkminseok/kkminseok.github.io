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

