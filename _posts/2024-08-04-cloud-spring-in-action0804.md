---
title: 클라우드 네이티브 인 액션(8) - API 게이트웨이, 서킷브레이커
author: minseok
date: 2024-08-04 00:02:00 +0800
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


이번장에서는 
- API 요청을 한 곳에서 받으며 보안, 모니터링 등 공통으로 발생하는 이슈를 다루는데에 사용하는 API 게이트웨이를 구현하고 복원력 향상을 위한 서킷브레이커를 구현한다.
- 웹 세션을 저장하기 위한 스프링 세션 데이터 레디스(Spring Session Data Redis)를 사용한다.
- 인그레이스를 이용한 쿠버네티스 관리
학습한다.

요청 앞단에서 받아서 공통적인 로직을 처리하는 서버를 **엣지 서버(Edge Server)**라고 불리는데 공통적인 기능을 처리하기 좋지만 빌드 및 배포 구성요소가 하나 더 늘어나고 네트워크 홉을 새롭게 추가하기에 결과적으로는 응답시간을 늘린다. 또한 단일 실패지점이 될 수 있어서 이중화 구조는 필수가 된다.

## Spring Cloud Gateway 설정

### 의존성 설정

```gradle
implementation 'org.springframework.cloud:spring-cloud-starter-gateway'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
```

### 네티서버 설정

```yml
server:
  port: 9000
  netty:
    connection-timeout: 2s
    idle-timeout: 15s
  shutdown: graceful

spring:
  application:
    name: edge-service
  lifecycle:
    timeout-per-shutdown-phase: 15s
```

서버 이름, 포트, 타임아웃, 등을 설정해준다.

스프링 게이트웨이는 클라이언트로 요청이 오면 요청이 정상이라고 판단되면 HandlerMapping을 거쳐 WebHandler로 보내지고 WebHadler는 일련의 필터를 통해 요청을 실행하게 된다.

필터체인은 Spring MVC와 동일하게 요청 전 필터와 요청 후 응답에 대한 필터 2가지로 나뉘어진다.

### 라우트 등록

```yml
spring:
  cloud:
    gateway:
      routes:
        - id: catalog-route
          uri: ${CATALOG_SERVICE_URL:http://localhost:9001}/books
          predicates:
            - Path=/books/**
        - id: order-route
          uri: ${ORDER_SERVICE_URL:http://localhost:9002}/orders
          predicates:
            - Path=/orders/**
```

uri에 있는 표현식은 환경변수에 값이 있으면 해당 값을 쓰고, 없으면 http://localhost~를 쓰게끔 설정하였다.
- /books로 시작하는 요청이면 catalog-route를 통하고 /orders로 시작하는 요청이면 order-route로 통할 것이다.

catalog-service, order-service를 띄우고 

```sh
http :9000/books
http :9000/orders
```

요청을 보내, 성공하면 된다.

장애에 대한 탄력적으로 대처할 수 있도록 요청에 대한 타임아웃 설정을 해줄 것이다.
또한 네티 서버는 요청이 증가함에 따라 그에 맞춰서 연결 수도 동적으로 늘리는데 이를 제어할 수도 있다.

### 타임아웃 설정, 연결풀 제한

```yml
spring:
  cloud:
    gateway:
      httpclient:
        connect-timeout: 2000 # 연결 수립까지의 제한 시간
        response-timeout: 5s # 응답받을때까지 기다리는 시간
        pool:
          type: elastic # 연결 풀 유형 elastic, fixed, disabled
          max-idle-time: 15s # 통신 채널이 닫기 전 기다리는 시간
          max-life-time: 60s # 통신 채널이 열러있는 시간
```

스프링 게이트웨이에서의 필터를 통해 요청, 응답에 대한 특별한 것들을 적용할 수 잇다.

요청시에는 인증, 서킷블레이커, 사용률 제한, 재시도 타임아웃 정의 등을 지정할 수 있고 응답시에는 보안 헤더 적용, 응답 본문에서 민감한 정보 수정 등 작업을 처리할 수 있다.

### 재시도 필터 사용

```yml
spring:
  cloud:
    gateway:
      default-filters: # 기본 필터 등록
        - name: Retry # 필터 이름
          args:
            retries: 3 # 최대 재시도 횟수
            methods: GET # GET 요청에 대해서만 적용
            series: SERVER_ERROR # 5xx에러에 대해서만 적용
            exceptions: java.io.IOException, # 해당 예외가 발생할 경우 적용
              java.util.concurrent.TimeoutException
            backoff: # 백오프 전략 적용
              firstBackoff: 50ms 
              maxBackoff: 500ms
              factor: 2
              basedOnPreviousValue: false
```

재시도 패턴은 특정 서버가 임시로 중단되었을 경우 유용하다.

하지만 이런 상태가 계속 지속 된다면 서비스가 확실히 정상으로 돌아오기 전까지 요청을 중단할 수 있다.
이를 도와주는 것이 **서킷 브레이커**이다.

나름의 동작 방식이 있는데 궁금한 분들은 한 번 찾아보길..

뒤에 설명을 위해 간단하게 말하자면 애플리케이션은 서킷브레이커에 의해 개방, 반개방, 폐쇄의 상태를 갖는데

- 폐쇄: 서비스간 요청이 가능한 상태
- 개방: 서비스간 요청이 불가능한 상태(에러 등으로 인하여)
- 반개방: 개방의 상태에서 몇분의 시간이 지나면 요청 보내려던 서비스에 요청을 보내서 정상적으로 서비스가 동작하는지 확인하는 상태
로 나뉠 수 있다.


## 서킷브레이커 적용

스프링에서 제공하는 스프링 클라우드 서킷 브레이커를 적용한다. 먼저 의존성을 추가해준다.

`reilience4j`의 리액티브 버전을 사용할 것이다.
```yml
implementation 'org.springframework.cloud:spring-cloud-starter-circuitbreaker-reactor-reilience4j'
```

다음은 라우트 설정에 추가해준다.

```yml
spring:
  cloud:
    gateway:
      routes:
        - id: catalog-route
          uri: ${CATALOG_SERVICE_URL:http://localhost:9001}/books
          predicates:
            - Path=/books/**
          filters:
            - name: CircuitBreaker # 필터 이름
              args:
                name: catalogCircuitBreaker  # 서킷 브레이커 이름
                fallbackUri: forward:/catalog-fallback # 문제가 발생한 경우 해당 uri로 요청 전달
        - id: order-route
          uri: ${ORDER_SERVICE_URL:http://localhost:9002}/orders
          predicates:
            - Path=/orders/**
          filters:
            - name: CircuitBreaker
              args:
                name: orderCircuitBreaker
```

cataloger-route에 대해서는 폴백 URI를 지정하였고, order-route에 대해서는 지정하지 않았다.
이를 통해서 각각의 상황에 대해 어떤방식으로 동작하는지 확인할 수 있다.


이제 서킷브레이커 자체를 설정해야한다.

빈으로 작성도 가능하고 `yml`파일로 작성도 가능한데, 책 예제에서는 yml파일로 작성하여서 작성해본다.


```yml
resilience4j:
  circuitbreaker:
    configs:
      default: # 모든 서킷 브레이커에 대한 기본 설정
        slidingWindowSize: 20 # 폐쇄 상태일 때 호출의 결과를 기록하는데 사용하는 슬라이딩 윈도우 크기
        permittedNumberOfCallsInHalfOpenState: 5 # 반개방 상태일 때 허용되는 호출 수
        failureRateThreshold: 50 # 실패율이 임계값 이상이면 회로는 개방상태로 변경
        waitDurationInOpenState: 15000 # 개방 상태에서 반개방 상태로 가기까지 기다릴 시간(밀리초)
  timelimiter:
    configs:
      default: # 모든 시간 제한에 대한 기본 설정
        timeoutDuration: 5s # 타임아웃 설정 (초)
```

### 폴백 Rest API 정의

위의 설정에서 catalog-service는 기본 필터인 Retry도 해당 라우트에 적용되어 있으므로 재시도에 대한 실패도 fallbackURI로 요청이 전달 될 것이다.

API를 정의하는데에는 `@RestController`로 정의가 가능하지만 책에서는 함수적 선언을 통해 API를 제작한다고 한다.


```java
package com.polarbookshop.edgeservice.web;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpStatus;
import org.springframework.web.reactive.function.server.RouterFunction;
import org.springframework.web.reactive.function.server.RouterFunctions;
import org.springframework.web.reactive.function.server.ServerResponse;
import reactor.core.publisher.Mono;

@Configuration
public class WebEndpoints {

    @Bean
    public RouterFunction<ServerResponse> routerFunction() {
        return RouterFunctions.route()
                .GET("/catalog-fallback", request ->
                        ServerResponse.ok().body(Mono.just(""), String.class))
                .POST("/catalog-fallback", request->
                        ServerResponse.status(HttpStatus.SERVICE_UNAVAILABLE).build())
                .build();
    }
}
```

`GET` 요청에 대해서는 폴백은 빈 문자열을 반환하고, `POST`요청에 대해서는 503을 반환한다.

책에서는 이렇게 되어있지만 상황에 따라서는 기본 값을 리턴한다던가, 캐시된 데이터를 반환하는 등의 작용을 할 수 있다.

### 테스트

이에 대한 테스트로는 **아파치 벤치마크**를 이용하여 하게 된다.

catalogs-service, order-service가 띄워져있으면 테스트가 불가능하기에 서버가 띄워져있으면 내려놓고 테스트를 진행한다.

```sh
> ab -n 21 -c 1 -m POST http://localhost:9000/orders    

...
Concurrency Level:      1
Time taken for tests:   0.240 seconds
Complete requests:      21
Failed requests:        12
   (Connect: 0, Receive: 0, Length: 12, Exceptions: 0)
Non-2xx responses:      21

```

서비스가 내려가 있기에 모든 요청에 대하여 오류가 발생해야한다.

설정에서 20차례 요청 이후에는 회로는 개방 상태로 전환하기 때문에 서버쪽 로그를 확인하면

```sh
Event FAILURE_RATE_EXCEEDED published: 2024-08-14T13:05:14.754711+09:00[Asia/Seoul]: CircuitBreaker 'orderCircuitBreaker' exceeded failure rate threshold. Current failure rate: 100.0
```

라는 에러를 확인할 수 있다.

20번째 요청에서 실패 임계값을 초과하였기 때문에 FAILURE_RATE_EXCEEDED 이벤트가 기록되고 그 바로 밑에 회로를 개방하는 이벤트를 발생한다.

```sh
STATE_TRANSITION published: 2024-08-14T13:05:14.755777+09:00[Asia/Seoul]: CircuitBreaker 'orderCircuitBreaker' changed state from CLOSED to OPEN
```

21번째 요청에서는 아예 요청을 하지 않는 것을 볼 수 있다.

```sh
Event NOT_PERMITTED published: 2024-08-14T13:05:14.757239+09:00[Asia/Seoul]: CircuitBreaker 'orderCircuitBreaker' recorded a call which was not permitted.
```

재시도와 폴백이 모두 구성되었을때 어떤 현상이 일어나는지 확인하기 위해 다른 라우터로 요청 테스트를 진행해본다.

```sh
> ab -n 21 -c 1 -m GET http://localhost:9000/books  

Complete requests:      21
Failed requests:        0
```

요청이 모두 폴백의 엔드포인트로 전달되었기에 클라이언트에는 어떤 오류도 발생하지 않았다.

참고로 다른 서비스 서버가 실행된 것이 아니기에 `ConnectException`에러가 발생한 것인데 이에 대한 재시도 처리 로직은 없기에 재시도 없이 서킷 브레이커와 폴백이 결합된 작동을 보여준다.

## 레디스를 통한 사용률 제한

시스템을 견고하고 복원력을 높이는데에 트래픽 사욜률 제한 기능도 필요하다. 

### 레디스 컨테이너 실행

각 사용자가 초당 최대 10개의 API를 호출할 수 있다고 할 때 이러한 요구사항을 구현하려면 각 사용자가 매초 수행하는 요청 수를 추적하는 스토리지 매커니즘이 필요하다.

이러한 요구사항을 지키는데에는 크기가 작고 일시적(10개의 요청 사항만 저장)이기 때문에 애플리케이션 메모리에 저장할 수 있다.

하지만 애플리케이션 메모리에 저장하면 인스턴스가 늘어날때마다 전체 시스템에 대한 요구사항이 아닌 각 인스턴스에 대한 요구사항으로 적용될 수 있다.

메모리기반의 저장소인 `Redis`를 사용하면 이러한 요구사항을 해결할 수 있다.

레디스 컨테이너를 실행하기 위해 docker-compose를 수정한다.

```yml
services:
  ...
  polar-redis:
  image: "redis:7.0"
  container_name: "polar-redis"
  ports:
    - 6379:6379
```

`docker compose up -d polar-redis`로 실행해준다.

스프링과의 통합을 위해 스프링에서 의존성을 추가해준다.

```gradle
implementation 'org.springframework.boot:spring-boot-starter-data-redis-reactive'
```

레디스에도 타임아웃 등을 설정해주기 위해 properties에도 내용을 추가해준다.

```yml
spring:
  data:
    redis:
      connect-timeout: 2s
      host: localhost
      port: 6379
      timeout: 1s
```

다음은 요청 사용률 제한을 위해 `RequestReteLimiter`라는 새로운 필터를 만든다.

해당 필터는 **토큰 버킷 알고리즘**을 기반으로 동작한다. 


> 토큰 버킷 알고리즘에 대해서는 제 블로그 글에도 정리 되어있습니다.
[처리율 제한 알고리즘에 대하여](https://kkminseok.github.io/posts/2022-12-12-InterviewBook04/)
{: .prompt-info}

각 제한에 대해서는 properties에서 처리가 가능하다.

```yml
spring:
  cloud:
    gateway:
      default-filters:
        - name: RequestRateLimiter
          args:
            redis-rate-limiter:
              replenishRate: 10 # 초당 버킷에 떨어지는 토큰의 수
              burstCapacity: 20 # 최대 20개 요청까지 허용 
              requestedTokens: 1 # 하나의 요청 처리에 몇 개의 토큰이 사용되는지 지정
```

요청률 제한 설정에서 적합한 값을 찾기위한 공식은 없다. 보통 애플리케이션 요구 사항에 맞춰서 조절하게 되고, 시행착오를 거치게 된다.

스프링 클라우드 게이트웨이는 레디스를 사용해 초당 발생하는 요청 수를 추적하게 된다. 사용자마다 요청 수를 보관해야하지만 아직 인증 매커니즘이 없기에 일단은 사용자 상관없이 단일 버킷을 사용하게 된다.

참고로 레디스가 문제가 발생해 사용할 수 없게 되면 게이트웨이는 사용률 제한을 일시적으로 비활성화 하게된다.

위에서 작성한 필터는 `KeyResolver` 빈을 통해 요청에 대해 사용할 버킷을 결정한다. 기본 설정은 인증된 사용자를 버킷으로 사용하도록 되어있는데 인증 구현이 추가 되기 전에 이에 대한 값을 수정해야한다. 따라서 관련 클래스를 만들어서 설정을 추가해준다.

```java
package com.polarbookshop.edgeservice.config;

import org.springframework.cloud.gateway.filter.ratelimit.KeyResolver;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import reactor.core.publisher.Mono;

@Configuration
public class RateLimiterConfig {
    
    @Bean
    public KeyResolver keyResolver() {
        return exchange -> Mono.just("anonymous");
    }
}
```

이렇게 설정하면 요청시 스프링 클라우드 게이트웨이가 요청 처리에 대한 정보를 헤더에 담고 있는 것을 확인할 수 있다.

```sh
> http :9000/books
Content-Length: 0
Content-Type: text/plain;charset=UTF-8
X-RateLimit-Burst-Capacity: 20
X-RateLimit-Remaining: -1
X-RateLimit-Replenish-Rate: 10
X-RateLimit-Requested-Tokens: 1
```

이 정보를 그대로 이용할 수도 있지만 보안적으로 문제가 발생할 수도 있기에 헤더 이름을 변경할 수 있다.  `spring.cloud.gateway.redis-rate-limiter`속성 그룹을 통해 비활성화 할 수 있다.

## 분산 세션 관리

레디스의 또 다른 용례로 분산 세션관리가 있다.

인증절차가 추가되고 gateway서버가 이중화 그 이상의 구조로 동작한다면 각 서버마다 사용자에 대한 인증정보를 서로 공유해야한다. 그러지 않으면 다른 서버로 요청이 들어갈때마다 인증절차를 다시 거쳐야할 수도 있다. 애플리케이션은 상태를 가지지 않으므로 외부 데이터 서비스에 해당 값을 저장하여 서로 공유하여 사용하도록 해야한다.


### 스프링 세션 데이터 레디스 사용

레디스는 세션관리에 널리 사용되는 옵션이고 스프링 세션 데이터 레디스를 통해 스프링과 통합이 가능하다.

이를 사용해보기 위해서 gradle에 의존성을 추가한다.

```gradle
ext {
  ...
  set('testcontainersVersion', "1.17.3")
}

dependencies { 
  implementation 'org.springframework.session:spring-session-data-redis'
  testImplementation 'org.testcontainers:junit-jupiter'
}

dependencyManagement {
  imports{
    mavenBom "org.testcontainers:testcontainers-bom:${testcontainersVersion}"
  }
}

```

테스트를 위해서 테스트컨테이너 관련 의존성도 추가하였다.

이에 맞춰서 추가로 서버 설정도 수정해준다.

```yml
spring:
  session:
    store-type: redis # 레디스를 사용하도록 설정
    timeout: 10m # 세션에 대한 타임아웃 default=10
    redis: 
      namespace: polar:edge # 모든 세션 데이터 앞에 붙일 고유한 네임스페이스
```

서비스 요청에 대하여 세션을 레디스에 저장하기 위해서 **필터**를 또 등록해줘야한다.
**SaveSession** 필터를 통해 세션정보가 레디스에 저장되도록 설정한다.

```yml
spring:
  cloud:
    gateway:
      default-filters:
        - SaveSession
```

### 세션 데이터 저장 확인 테스트 진행

레디스가 웹 세션 관련 데이터를 저장 시 스프링 콘택스트가 올바르게 로드 되는지 확인해야한다.

```java
package com.polarbookshop.edgeservice;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.DynamicPropertyRegistry;
import org.springframework.test.context.DynamicPropertySource;
import org.testcontainers.containers.GenericContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

@SpringBootTest(
        webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT
)
@Testcontainers // 테스트 컨테이너 자동시작 ,종료 알림
class EdgeServiceApplicationTests {

    private static final int REDIS_PORT = 6379;

    // 테스트를 위한 레디스 컨테이너 정의
    @Container
    static GenericContainer<?> redisContainer = new GenericContainer<>("redis:7.0")
            .withExposedPorts(REDIS_PORT);

    @DynamicPropertySource
    static void redisProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.redis.host", () -> redisContainer.getHost());
        registry.add("spring.redis.port", () -> redisContainer.getMappedPort(REDIS_PORT));
    }

    @Test
    void verifyThatSpringContectLoads() {
    }

}
```

이전에도 비슷한 코드를 작성하였고, 테스트가 잘 진행된다면 아직까지는 문제가 없는 것이다.


## 쿠버네티스 인그레스를 통한 외부 액세스 관리

현재 쿠버네티스로 다른 서비스들은 배포 되고 있는데 위의 서비스는 배포 됨뿐만 아니라 외부 접근점도 가지고 있기에 이에 따른 추가 설정이 필요하다.
쿠버네티스 인그레스를 통해서 외부와의 통신점을 연결해줘야한다.













