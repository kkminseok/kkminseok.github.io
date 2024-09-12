---
title: 클라우드 네이티브 인 액션(9) - 래빗MQ, 스프링 클라우드 스트림, 스프링 클라우드 함수
author: minseok
date: 2024-08-16 00:02:00 +0800
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


이번 절에는 이벤트 중심 설계에대한 이야기와 이를 중심으로 래빗 MQ 및 클라우드 스트림, 함수를 적용해볼 예정이다.

클라이언트가 주문을 하면 주문 접수에 대한 알림은 이벤트 브로커에 보내고, 이벤트 브로커는 배송 서비스에 접수한 주문에 대한 알림을 준다. 그 이후 배송 서비스는 주문을 배송하고 발송된 오더에 대한 알림을 다시 주문서비스에 전달하여 주문서비스는 상태를 업데이트 하게 된다.

## 발행/구독을 위한 래빗MQ적용

docker-compose를 정의하여 RabbitMQ를 띄운다.


```docker
services:
  polar-rabbitmq:
    image: rabbitmq:3.10-management
    container_name: polar-rabbitmq
    ports:
      - 5672:5672
      - 15672:15672
    volumes:
      - ./rabbitmq/rabbitmq.conf:/etc/rabbitmq/rabbitmq.conf
```

현재디렉터리의 /rabbitmq/rabbitmq.conf를 마운트 해줬으므로 해당 경로에 파일도 작성한다.

```sh
mkdir rabbitmq
cd tabbitmq
touch rabbitmq.conf

# rabbitmq.conf 내용
default_user=user
default_pass=password
```

이후, `doceker-compose up -d polar-raabbitmq`를 통해 RabbitMQ를 실행해주고 localhost:15672에 접근하며 빈 페이지가 뜨지 않는다면 성공이다.

![](/assets/img/cloudNativeSpringInAction/10_rabbitmq.png)

## 스프링 클라우드 함수 적용

스프링 클라우드 함수를 사용하여 공급자, 함수, 소비자 측면에서의 주문 흐름 비즈니스 로직을 작성한다. 이를 통해서 메시지를 어떻게 처리할 것인지에 대한 논리를 정의할 수 있다.

배송서비스를 예를들어서 클라이언트가 주문을 하게 되면 배송서비스는 주문을 포장하고 라벨을 붙이는 작업을 하게 된다.또한 이 작업을 마무리하고 배송을 시작하게되면 클라이언트에게 배송에 대한 알림을 보낼 책임이 있다. 포장과 레이블 작업이 애플리케이션단에서 수행된다고 가정하면

```text
주문접수 -> pack() -> label() -> 주문배송
```

이런식으로 함수로 구성할 수 있다.

`pack()`함수는 주문Id를 입력받아서 포장에 대한 처리를 진행하고 주문Id를 반환한다.
`label`()함수는 주문Id를 받아서 라벨링을 진행하고 마찬가지로 주문Id를 반환하여 출력하게 된다.

이를 기준으로 스프링 클라우드 함수를 이용하여 구현해보도록 하자

배송 서비스에 대한 스프링 프로젝트를 시작하여야한다.

### 프로젝트 설정 적용

스프링 클라우드 함수를 사용하기 위한 의존성을 추가한다.

```gradle
implementation 'org.springframework.boot:spring-boot-starter'
implementation 'org.springframework.cloud:spring-cloud-function-context'
testImplementation 'org.springframework.boot:spring-boot-starter-test'
```


이에 맞춰서 애플리케이션 설정도 추가해준다.

```yml
spring:
  application:
    name: dispatcher-service
server:
  port: 9003
```

웹서버를 내장하고 있지만 추후 모니터링 시스템을 추가할 것이기에 서버 포트를 따로 지정하였다. 

### DTO작성, 비즈니스 로직 작성

앞에서 `pack()`함수는 주문 Id를 입력으로 받을 것이므로 이에 대한 DTO를 작성해준다.

```java
package com.nhn.corp.ext.dispatcherservice;

public record OrderAcceptedMessage(
        Long orderId
) {}
```

다음은 `pack()`함수를 정의한다. 예제에서는 간단하게 로그로 출력하여 포장에 대한 완료를 나타낸다.

```java
@Configuration
public class DispatchingFunctions {

    private static final Logger log = LoggerFactory.getLogger(DispatchingFunctions.class);

    @Bean
    public Function<OrderAcceptedMessage, Long> pack() {
        return orderAcceptedMessage -> {
            log.info("포장이 완료되었습니다. 주문 Id :{}", orderAcceptedMessage.orderId());
            return orderAcceptedMessage.orderId();
        };
    }
}
```

참고로 `@Configuration`과 `@Bean`을 사용하지 않으면 스프링을 전혀 사용하지 않은 순수 자바8 문법으로 작성된 코드다.

저 둘 애노테이션을 작성한 이유는 스프링 클라우드 함수 때문이다.

스프링 클라우드 함수는 함수자체를 빈으로 등록하면 이를 인식하여 관리할 수 있다.
또한, 스프링 클라우드 함수는 명령형과 리액티브 코드 모두 지원한다. `label()`함수는 리액티브 코드로 작성을 해볼 것이다.

마찬가지로 DTO와 비즈니스 로직을 작성해본다.

### 리액티브 형식 DTO, 비즈니스 로직 작성

```java
//DTO
package com.nhn.corp.ext.dispatcherservice;

public record OrderDispatcherMessaged(
        Long orderId
) {}
```

```java
//Business 
@Bean
public Function<Flux<Long>, Flux<OrderDispatcherMessaged>> label() {
    return orderFlux -> orderFlux.map(orderId -> {
        log.info("라벨링이 완료되었습니다. 주문 Id: {}", orderId);
        return new OrderDispatcherMessaged(orderId);
    });
}
```

주문Id를 입력받고 해당 주문Id에 대한 객체로 반환하는 코드를 작성했다.

이제 이를 어떻게 사용할 수 있을까에 대해 생각해봐야한다.

먼저, 이 둘을 합성해야한다. pack() -> label()이라는 순서가 지켜져야하며, Java8에서는 `andThen()`, `compose()`함수를 이용해서 함수를 순서에 맞춰 합성이 가능하다.

문제는 다음에 호출될 함수의 입력유형과 그 전에 호출된 함수의 출력유형이 같아야한다.

스프링클라우드 함수는 이러한 문제를 해결해주며 위와같이 명령어 함수와 리액티브 함수의 합성을 내부적으로 변환을 수행하여 진행할 수 있게 한다.

이에 맞춘 설정을 진행해주자.

```yml
spring:
  application:
    name: dispatcher-service
  cloud: # 추가
    function:
      definition: pack|label
server:
  port: 9003
```

이를 통해서 스프링 클라우드 함수를 통해 이들 함수를 합성해서 새로운 함수를 생성할 수 있다.

이렇게 합성된 함수를 어떻게 호출할 수 있을까?

스프링 클라우드 함수가 지원하는 방식은 여러가지 있다. REST 엔드포인트로 노출도 가능하고 여러 클라우드 플랫폼에 배포하기 위해 애플리케이션을 패키징하고 플랫폼에서 제공하는 어댑터 중 하나를 사용하여 호출할 수도 있다. 또는 스프링 클라우드 스트림과 결합하여 RabbitMQ나 카프카와 같은 이벤트 브로커의 메시지 채널에 함수를 바인딩할 수도 있다.

해당 작업을 진행하기 전에 함수각각에 대한 테스트와 합성된 함수에 대한 테스트를 진행해야한다.

## 스프링 클라우드 함수 테스트 적용

스프링 클라우드 함수에서 제공하는 `@FunctionalSpringBootTest`를 통해 통합테스트를 진행할 수 있다.

일부 비즈니스 로직이 Reactor를 사용하였기에 Reactor에 대한 테스트 의존성을 추가해준다.

```gradle
testImplementation 'io.projectreactor:reactor-test'
```

다은은 테스트 코드를 작성해준다.

```java

@FunctionalSpringBootTest
public class DispatchingFunctionsIntegrationTests {

    @Autowired
    private FunctionCatalog catalog;

    @Test
    void packAndLabelOrder() {
        Function<OrderAcceptedMessage, Flux<OrderDispatcherMessage>> packAndLabel = catalog.lookup(
                Function.class,
            "pack|label"); // FunctionCatalog로 부터 합성 함수를 가져옴

        long orderId = 727;

        StepVerifier.create(packAndLabel.apply(
                new OrderAcceptedMessage(orderId) // 함수에 대한 입력인 OrderAcceptedMessage 정의
        ))
                .expectNextMatches(dispatchedOrder -> dispatchedOrder.equals(new OrderDispatcherMessage(orderId)))
                .verifyComplete();// 함수 출력의 객체가 OrderDispatcherMessage인지 확인
    }
}
```

비즈니스 로직만 구현하고 인프라 문제는 프레임워크에 위임하기 위한 간단하고 효과적인 방법이 함수이다. 다음은 스프링 클라우드 스트림을 사용해 RabbitMQ의 메시지 채널에 함수를 바인딩할 것이다.

## 스프링 클라우드 스트림을 통한 메시지 처리

스프링 클라우드 스트림에 대한 간단한 설명을 하자면 개발자는 비즈니스 로직에만 신경쓸 수 있도록 메시지브로커 통합 같은 인프라는 프레임워크가 담당하여 처리하여 도와주는 스프링의 프레임워크다.

이전 버전에서는 비즈니스 로직을 클라우드 스트림 구성요소와 일치시켜야하기에 코드를 수정해야할 일이 있었는데 현재는 그러지 않아도 된다는 장점 외에도 많은 장단점이 있으니 찾아보면 될 듯하다.

스프링 클라우드 스트림은 3가지 개념을 기반으로 구성되어있다.

- 대상 바인더: RabbitMQ나 카프카와 같은 외부 메시지 시스템과의 통합을 제공하는 컴포넌트
- 대상 바인딩: 외부 메시지 시스템 개체(큐, 토픽 등)와 애플리케이션의 생산자/소비자와 연결해주는 요소
- 메시지: 대상바인더, 더 나아가 외부 메시지 시스템과 통신하기 위해 애플리케이션의 생상자와 소비자가 사용하는 데이터 구조

이 세가지를 프레임워크 자체에서 처리된다. 그렇기에 비즈니스로직에서는 외부 메시징 시스템을 알지 못한다.

정리하자면 비즈니스 로직을 함수로 정의하고 스프링 클라우드 함수가 이를 관리하도록 설정한 다음, 사용하려는 브로커에 대한 스프링 클라우드 스트림 바인더 프로젝트 의존성을 추가하고 메시지 브로커를 통해 함수를 노출할 수 있다. 

### 스프링과 RabbitMQ 통합

```gradle
implementation 'org.springframework.cloud:spring-cloud-stream-binder-rabbit'
```

이에 맞춰서 애플리케이션 RabbitMQ통합설정도 추가해주자.

```yml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: user
    password: password
    connection-timeout: 5s
```

이후 애플리케이션이 정상적으로 동작하면 성공적으로 연동된 것이다.

스프링 클라우드 스트림에서 함수 프로그래밍 모델을 사용하면 입력 데이터를 받는 함수에 대해서는 입력 바인딩이, 출력 데이터를 반환하는 함수에 대해서는 출력 바인딩이 생성된다. 각 바인딩은 규칙에 따라 논리적 이름을 갖게 된다.

- 입력 바인딩: <functionName> + -in + <index>
- 출력 바인딩: <functionName> + -out + <index>

카프카에서처럼 파티션을 사용하지 않으면 index값은 항상 0이다. 

즉, 위에서 작성한 합성함수에 대해서 입력바인딩의 논리적 이름은 packlabel-in-0이 될 것이고, 출력바인딩은 packlabel-out-0이 될 것이다.

이는 스프링 클라우드 스트림에 대한 규칙이라 RabbitMQ입장에서는 이런 규칙을 모른다.

개발자가 바인딩 이름을 직접 지정할 수도 있다. 이를 지정해보는 예제를 작성해본다.

application.yml에 해당 정보를 넣어주면 된다.

### 클라우드 스트림 바인딩 및 RabbitMQ 대상 설정

```yml
spring:
  cloud:
    function:
      definition: pack|label
    stream:
      bindings:
        packlabel-in-0:
          destination: order-accepted
          group: ${spring.application.name}
        packlabel-out-0:
          destination: order-dispatched
```

참고로 스프링 클라우드 스트림은 관련 익스체인지나 큐가 없으면 애플리케이션이 부트할때 설정에 맞춰서 생성하게 된다.

출력 바인딩 packlabel-out-0은 RabbitMQ의 order-dispatch 익스체인지에 매핑된다.
입력 바인딩 packlabel-in-0은 RabbitMQ의 order-accepted 익스체인지 및 order-accepted.dispatcher-service 큐에 매핑된다. 

group(소비자 그룹)에 대해서 설정한 이유는 이중화와 같은 구조때문이다.

보통 복원력을 증진시키기 위해서 애플리케이션 인스턴스를 여러개 띄워놓는데, 하나의 입력에 대해서 모든 애플리케이션 인스턴스가 처리하도록 하면 안된다. 이는 일관성 문제와 오류를 발생시킬 위험이 있는데, 소비자 그룹을 지정하여 같은 그룹에 속한 소비자끼리 구독을 공유하게끔 한다. A서비스가 B서비스가 각각 복제본으로 실행되고 있을때 애플리케이션 이름으로 그룹을 설정하면 각각의 A서비스와 B서비스의 한 인스턴스에 의해서만 수신되고 처리될 수 있게 한다.

제대로 연동되었는지 확인하기 위해 테스트용 메시지를 전송해볼 것이다.

localhost:15672에 들어가서

![](/assets/img/cloudNativeSpringInAction/10_rabbitmq_test.png)

orderId를 임의로 설정한 뒤 Publish message를 클릭하여 테스트 메시지를 전송하면

```sh
2024-08-16T17:25:08.732+09:00  INFO 35169 --- [dispatcher-service] [tcher-service-1] o.s.a.r.c.CachingConnectionFactory       : Created new connection: rabbitConnectionFactory.publisher#4ee6b551:0/SimpleConnection@7c873b19 [delegate=amqp://user@127.0.0.1:5672/, localPort=61352]
2024-08-16T17:25:33.044+09:00  INFO 35169 --- [dispatcher-service] [tcher-service-1] c.n.c.e.d.DispatchingFunctions           : 포장이 완료되었습니다. 주문 Id :114
2024-08-16T17:25:33.047+09:00  INFO 35169 --- [dispatcher-service] [tcher-service-1] c.n.c.e.d.DispatchingFunctions           : 라벨링이 완료되었습니다. 주문 Id: 114
```

와 같은 로그를 볼 수 있다.

### 스프링 클라우드 바인더 통합확인 테스트 진행

외부 메시징과 통합을 테스트하기 위해 스프링 클라우드 스트림이 제공하는 테스트 바인더를 사용할 것이다.

의존성을 추가해야한다.

```gradle
testImplementation("org.springframework.cloud:spring-cloud-stream") {
		artifact {
			name = "spring-cloud-stream"
			extension = "jar"
			type ="test-jar"
			classifier = "test-binder"
		}
	}
```

> 만약 다른 의존성으로인해서 오류가 발생한다면 제 깃레포지토리 의존성을 참고해주세요
{: .prompt-info}



이후 테스트 절차는 

1. 테스트 바인더에 대한 설정을 제공하는 클래스 임포트
2. 입력 바인딩(packlabel-in-0)을 나타내는 빈을 주입
3. 출력 바인딩(packlabel-out-0)을 나타내는 빈을 주입

테스트코드를 작성해본다.

```java
package com.nhn.corp.ext.dispatcherservice;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.Test;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.cloud.stream.binder.test.InputDestination;
import org.springframework.cloud.stream.binder.test.OutputDestination;
import org.springframework.cloud.stream.binder.test.TestChannelBinderConfiguration;
import org.springframework.context.annotation.Import;
import org.springframework.integration.support.MessageBuilder;
import org.springframework.messaging.Message;
import java.io.IOException;

import static org.assertj.core.api.Assertions.assertThat;


@SpringBootTest
@Import(TestChannelBinderConfiguration.class)
public class FunctionsStreamIntegrationTests {

    @Autowired
    private InputDestination input;

    @Autowired
    private OutputDestination output;

    @Autowired
    private ObjectMapper objectMapper;

    @Test
    void whenOrderAcceptedThenDispatched() throws IOException {
        long orderId = 3;
        Message<OrderAcceptedMessage> inputMessage = MessageBuilder
                .withPayload(new OrderAcceptedMessage(orderId)).build();
        Message<OrderDispatchedMessage> expectedOutputMessage = MessageBuilder
                .withPayload(new OrderDispatchedMessage(orderId)).build();
        this.input.send(inputMessage);
        assertThat(objectMapper.readValue(output.receive().getPayload(), OrderDispatchedMessage.class))
                .isEqualTo(expectedOutputMessage.getPayload());

    }
}
```

## 스프링 클라우드 스트림을 통한 메시지 생성 및 소비

생산자와 소비자를 구현해볼 예정이다.

### 이벤트 소비자 구현 및 멱등성 문제

배송서비스 애플리케이션은 주문의 배송이 이뤄지면 메시지를 생성하기에 주문 서비스에서 이벤트를 생성해야한다.

주문 서비스 프로젝트에서 의존성을 추가해줘야한다.

```gradle
implementation 'org.springframework.cloud:spring-cloud-stream-binder-rabbit'
testImplementation("org.springframework.cloud:spring-cloud-stream")
```

이후 이벤트 소비자 DTO를 작성한다.

```java
package com.nhn.corp.ext.orderservice.order.event;

public record OrderDispatchedMessage (
        Long orderId
){ }
```

비즈니스 로직을 구현할 차례이다. 
주문이 발송되면 배송 서비스 애플리케이션이 생성한 메시지를 소비하는 함수를 구현한다. 
이 메시지는 메시지를 수신하고 데이터베이스 엔티티를 업데이트할 Consumer가 된다. 

```java
//OrderService.java
public Flux<Order> consumeOrderDispatchedEvent(Flux<OrderDispatchedMessage> flux) {
        return flux.flatMap(message -> orderRepository.findById(message.orderId()))
                .map(this::buildDispatchedOrder)
                .flatMap(orderRepository::save);
    }

private Order buildDispatchedOrder(Order existOrder) {
        return new Order(
                existOrder.id(),
                existOrder.bookIsbn(),
                existOrder.bookName(),
                existOrder.bookPrice(),
                existOrder.quantity(),
                OrderStatus.DISPATCHED, //배송됨
                existOrder.createdDate(),
                existOrder.lastModifiedDate(),
                existOrder.version()
        );
    }
```

```java
package com.nhn.corp.ext.orderservice.order.event;

import com.nhn.corp.ext.orderservice.order.domain.OrderService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import reactor.core.publisher.Flux;

import java.util.function.Consumer;

@Configuration
public class OrderFunctions {

    private static final Logger log = LoggerFactory.getLogger(OrderFunctions.class);

    @Bean
    public Consumer<Flux<OrderDispatchedMessage>> dispatchOrder(OrderService orderService) {
        return flux -> orderService.consumeOrderDispatchedEvent(flux) //각 발송된 메시지에 대해 데이터 베이스 업데이트
                .doOnNext(order -> log.info("The order with id {} is dispatched", order.id())) // 데이터베이스 업데이트시 로그성으로 남김
                .subscribe(); //리액티브 스트림을 활성화 하기 위한 구독. 구독자가 없으면 스트림을 통해 데이터가 흐르지 않음.
    }
}
```

구현할 때 유념해야할 부분은 래빗MQ가 적어도 하나의 전달을 보증하기 때문에 중복으로 메시지를 받을 수 있다. 하지만 비즈니스로직상 특정 주문의 상태를 DISPATCHED로 보장하는 멱등성을 지니기에 걱정할 부분은 없다. 

### 클라우드 스트림 바인딩 및 래빗 MQ 통합 설정

통합을 위해서 의존성을 추가하였는데 이에 맞춰서 서버 설정도 추가해야한다.

```yml
spring:
  cloud:
    function:
      definition: dispatchOrder
    stream: 
      bindings:
        dispatchOrder-in-0:
          destination: order-dispatched
          group: {spring.application.name}
  rabbitmq:
    host: localhost
    port: 5672
    username: user
    password: password
    connection-timeout: 5s
```

order-dispatched.order-service 큐 및 메시지 채널을 생성한다.

## 이벤트 생성자 구현 및 원자성 문제

공급자는 메시지 발원지다. 공급자는 소비자와 달리 명시적으로 활성화해야 동작한다.

다양한 방식으로 호출할 수 있지만 웹서비스를 제작하고 있으므로 Rest 엔드포인트를 사용하여 이벤트를 생성한다.
즉, 주문 서비스에서 사용자가 POST요청을 보내면 주문 수락을 알리는 메시지를 생성한다.

### DTO 모델링


```java
package com.nhn.corp.ext.orderservice.order.event;

public record OrderAcceptedMessage(
        Long orderId
) {}
```

다음은 비즈니스로직을 작성해야하는데, `StreamBridge`라는 객체를 사용할 것이다.
이 객체를 사용하면 REST 계층과 스트림을 연결할 수 있고 특정 대상에게 데이터를 보내는 명령을 수행할 수 있다. 이 객체에 더해 데이터를 정제하거나 로그와 같은 전처리도 가능하다.

```java
//OrderService.java

@Service
public class OrderService {
   ....
   private final StreamBridge streamBridge;


    public OrderService(...,StreamBridge streamBridge) {
        ...
        this.streamBridge = streamBridge;
    }


        public void publishOrderAcceptedEvent(Order order) {
        if (!order.status().equals(OrderStatus.ACCEPTED)){
            return ;
        }
        var orderAcceptedMessage = new OrderAcceptedMessage(order.id());
        log.info("Sending order accepted event with id: {}", order.id());
        var result = streamBridge.send("acceptOrder-out-0", orderAcceptedMessage);
        log.info("Result of sending data for order with id : {} :{}",order.id(), result);
    }
}
```

- publishOrderAcceptedEvent: 만약 접수된 상태가 아니라면 로직을 진행하지 않고, 메시지를 생성하여 StreamBridge 객체를 통하여 메시지를 acceptOrder-out-0 바인딩에 **명시적**으로 전달한다.


클라우드 스트림 출력 바인딩을 설정해주자.

```yml
spring:
  ... 
  stream:
    bindings:
      dispatchOrder-in-0:
        destination: order-dispatched
        group: {spring.application.name}
      acceptOrder-out-0:
        destination: order-accepted
```

이제 주문이 접수될 때마다 위에서 작성한 함수를 호출하게끔 하면 된다.

시스템의 일관성을 유지하기 위해서 데이터베이스에 데이터를 저장하는 것과 메시지를 전송하는것은 하나의 **트랜잭션**으로 처리되어야한다. 둘 다 성공하거나 둘 다 실패해야한다. 그래서 두 개의 작업을 하나의 트랜잭션으로 묶어야한다. 

때문에 기존에 작성했던 함수에서 `@Transactional` 애노테이션과 로직을 추가해준다.

```java
@Transactional //원자성 보장
public Mono<Order> submitOrder(String isbn, int quantity) {
    return bookClient.getBookByIsbn(isbn)
            .map(book -> buildAcceptedOrder(book,quantity))
            .defaultIfEmpty(buildRejectedOrder(isbn,quantity))
            .flatMap(orderRepository::save)
            .doOnNext(this::publishOrderAcceptedEvent); // 메시지 발송 추가
}
```

데이터베이스와 관련된 트랜잭션은 스프링 부트에서 미리 설정되어있지만 래빗MQ로 설정한 채널은 기본적으로 트랜잭션을 지원하지 않는다. 때문에 래빗MQ에 대한 트랜잭션을 지원하도록 yml에 내용을 추가해야한다.

```yml
spring:
  ...
  stream:
  bindings:
    dispatchOrder-in-0:
      destination: order-dispatched
      group: {spring.application.name}
    acceptOrder-out-0:
      destination: order-accepted
  rabbit: # 추가 구문
    bindings:
      acceptOrder-out-0:
        producer:
          transacted: true
```

이렇게 진행하면 스프링 클라우드 스트림, 래빗 MQ에 대한 통합이 완료되었다.

실제 테스트하는 방법은 기존 프로젝트 기반요소가 많이 포함되어 있어서 적는게 도움이 될 지 모르지만..

```sh
# 카탈로그 생성
> http POST :9001/books author="kms" \
title="Rabbit MQ Message Test" isbn="1234567896" \
price=9.90 publisher="minseok-kang"

# 책 3권 주문
> http POST :9002/orders isbn=1234567896 quantity=3

# 주문 상태 조회 - 상태가 DISPATCHED여야만 함.
> http :9002/orders

 {
        "bookIsbn": "1234567896",
        "bookName": "Rabbit MQ Message Test-kms",
        "bookPrice": 9.9,
        "createdDate": "2024-08-17T08:37:56.317637Z",
        "id": 3,
        "lastModifiedDate": "2024-08-17T08:37:56.513087Z",
        "quantity": 3,
        "status": "DISPATCHED",
        "version": 2
    },



```






