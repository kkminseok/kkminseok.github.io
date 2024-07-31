---
title: 클라우드 네이티브 인 액션(7) - 리액티브 스프링 사용
author: minseok
date: 2024-07-30 00:02:00 +0800
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

저번장에서는 k8s를 사용해서 배포를 진행하였다. 트래픽이 좀 더 많아지면 같은 요청당 쓰레드 모델로는 한계에 봉착하게 된다.

이를 극복하기 위해서 비동기적,논 블럭킹 방식의 리액티브 애플리케이션을 개발해야한다.

이번 장에서는 스프링 웹 플럭스, 스프링 데이터 R2DBC를 사용할 것이다.

리액티브 애플리케이션은 이벤트 기반으로 비동기적으로 요청들을 처리하게 되는데, 간단하게 말하면 스레드가 작업을 완료될 때까지 계속 기다리지 않고 작업을 수행하다가 작업이 완료되면 해당 작업에 대한 처리를 하게 된다.

R2DBC와 스프링 웹플럭스를 이용한 주문 서비스를 만들어본다.

## □ 리액티브 애플리케이션 부트스트래핑

스프링 부트 프로젝트를 만들고 의존성을 추가해준다.

```gradle
    implementation 'org.springframework.boot:spring-boot-starter-data-r2dbc'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    implementation 'org.springframework.boot:spring-boot-starter-webflux'
    implementation 'org.flywaydb:flyway-database-postgresql'

    runtimeOnly 'org.flywaydb:flyway-core'
    runtimeOnly 'org.postgresql:postgresql'
    runtimeOnly 'org.postgresql:r2dbc-postgresql'
    runtimeOnly 'org.springframework:spring-jdbc'

    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'io.projectreactor:reactor-test'
    testImplementation 'org.testcontainers:junit-jupiter'
    testImplementation 'org.testcontainers:postgresql'
    testImplementation 'org.testcontainers:r2dbc'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
```

이후 `application.yml`을 수정하여 서버에 대한 설정도 해준다.

```yml
server:
  port: 9002
  shutdown: graceful
  netty:
    connection-timeout: 2s
    idle-timeout: 15s

spring:
  application:
    name: order-service
  lifecycle:
    timeout-per-shutdown-phase: 15s
```

- 우아한종료
- TCP 연결 타임아웃 2초
- TCP 연결닫기 전에 기다리는시간 15초
- 15초간의 우아한 종료기간을 
정의하였다.

## □ R2DBC사용을 위한 DB docker-compose 작성

DB를 사용하도록 docker-compose를 작성해준다.

먼저 컨테이너가 뜰때 스키마를 만들어주도록 init.sql을 작성한다.

```sql
CREATE DATABASE polardb_catalog;
CREATE DATABASE polardb_order;
```

이후 docker-compose 파일에서 해당 sql을 컨테이너 실행시 작성할 수 있도록 마운트를 해준다.

```yml
....
  polar-postgres:
    image: "postgres:14.12"
    container_name: "polar-postgres"
    ports:
    - 15432:5432
    environment:
    - POSTGRES_USER=user
    - POSTGRES_PASSWORD=password
    volumes:
      - ./postgresql/init.sql:/docker-entrypoint-initdb.d/init.sql
```

이후 `docker-compose up -d polar-postgres` 명령어를통해서 컨테이너를 실행해준다.

또한 Spring 에서도 설정을 해당 DB를 바라보도록 수정해준다.

```yml
#application.yml
spring:
  r2dbc:
    username: user
    password: password
    url: r2dbc:postgresql://localhost:15432/polardb_order
    pool:
      max-create-connection-time: 2s
      initial-size: 5
      max-size: 10
  flyway:
    user: ${spring.r2dbc.username}
    password: ${spring.r2dbc.password}
    url: jdbc:postgresql://localhost:15432/polardb_order
```

flyway는 버전관리를 위해 추가해놓았는데, 아직 R2DBC를 지원하지 않아서 jdbc드라이버를 통해서 연결해야한다.

## □ 지속성 엔티티 정의

```java
package com.nhn.corp.ext.orderservice.order.domain;

import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.Id;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.annotation.Version;
import org.springframework.data.relational.core.mapping.Table;

import java.time.Instant;

@Table("orders")
public record Order(

        @Id
        Long id,

        String bookIsbn,
        String bookName,
        Double bookPrice,
        Integer quantity,
        OrderStatus status,

        @CreatedDate
        Instant createdDate,

        @LastModifiedDate
        Instant lastModifiedDate,

        @Version
        int version
) {
    public static Order of(String bookIsbn, String bookName, Double bookPrice, Integer quantity, OrderStatus status) {
        return new Order(null, bookIsbn, bookName, bookPrice, quantity, status, null, null, 0);
    }
}


public enum OrderStatus {
    ACCEPTED,
    REJECTED,
    DISPATCHED
}

```

또한 데이터감사 기능을 활용하기 위해서 `DataConfig.java`파일도 작성해준다.

```java
package com.nhn.corp.ext.orderservice.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.r2dbc.config.EnableR2dbcAuditing;

@Configuration
@EnableR2dbcAuditing
public class DataConfig {
}
```

리액티브 레포지토리는 JPA처럼 추상화된 것을 사용하면 된다.

```java

public interface OrderRepository extends ReactiveCrudRepository<Order, Long> {
}

```

## □ 비즈니스 로직 구성

간단한 CRUD를 작성한다.

```java
package com.nhn.corp.ext.orderservice.order.domain;

import org.springframework.stereotype.Service;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@Service
public class OrderService {

    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public Flux<Order> getAllOrders() {
        return orderRepository.findAll();
    }

    public Mono<Order> submitOrder(String isbn, int quantity) {
        return Mono.just(buildRejectedOrder(isbn, quantity))
                .flatMap(orderRepository::save);
    }

    public static Order buildRejectedOrder(String bookIsbn, int quantity) {
        return Order.of(bookIsbn, null, null, quantity, OrderStatus.REJECTED);
    }

}
```

## □ 컨트롤러 작성

먼저 Request 모델을 작성한다.

```java
package com.nhn.corp.ext.orderservice.order.web;

import jakarta.validation.constraints.Max;
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;

public record OrderRequest(
        @NotBlank(message = "The book ISBN must be defined.")
        String isbn,

        @NotNull(message = "The book quantity must be defined.")
        @Min(value = 1, message = "You must order at least 1 item.")
        @Max(value = 5, message = "You cannot order more than 5 items.")
        Integer quantity
){}
```

이후 컨트롤러를 작성한다.

```java
package com.nhn.corp.ext.orderservice.order.web;

import com.nhn.corp.ext.orderservice.order.domain.Order;
import com.nhn.corp.ext.orderservice.order.domain.OrderService;
import jakarta.validation.Valid;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("orders")
public class OrderController {
    private final OrderService orderService;

    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @GetMapping
    public Flux<Order> getAllOrders(){
        return orderService.getAllOrders();
    }

    @PostMapping
    public Mono<Order> submitOrder(@RequestBody @Valid OrderRequest orderRequest) {
        return orderService.submitOrder(orderRequest.isbn(), orderRequest.quantity());
    }
}

```

데이터 검증도 추가된 코드이며 테스트를 해보면 잘 된다.

```bash
http POST :9002/orders isbn=1234567890 quantity=3
HTTP/1.1 200 OK
Content-Length: 203
Content-Type: application/json

{
    "bookIsbn": "1234567890",
    "bookName": null,
    "bookPrice": null,
    "createdDate": "2024-07-30T12:51:42.981231Z",
    "id": 1,
    "lastModifiedDate": "2024-07-30T12:51:42.981231Z",
    "quantity": 3,
    "status": "REJECTED",
    "version": 1
}
```

## □ 웹 클라이언트 사용

HTTP 요청을 수행하기 위한 클라이언트를 번들로 제공하는데 크게 2가지가있다.
RestTemplate과 WebClient인데, RestTemplate은 업데이트가 되지 않아서 실질적으로 중단된 상태로 있고 WebClient는 이러한 RestTemplate의 대안으로 나왔다. 

다른 서비스를 호출하기 위해서 해당 서비스의 URI정보를 가지고 있어야하는데, 보통 설정으로 따로 관리하기에 `ConfigurationProperties`어노테이션을 사용해 관리하도록 한다.

```java
//ClientProperties.java
@ConfigurationProperties(prefix = "polar")
public record ClientProperties(@NotNull
                               URI catalogServiceUri) {
}



//OrderServiceApplication.java
@SpringBootApplication
@ConfigurationPropertiesScan // <--
public class OrderServiceApplication {
    ...
}

//application.yml
polar:
  catalog-service-uri: "http://localhost:9001"
```

주문 서비스와 카탈로그 서비스 모두 book이라는 객체를 가지고 있는데, 이 둘을 관리하려면 어떻게 해야할까? 공유 라이브러리로 만들어서 관리하면 두 서비스간의 결합도가 높아지고, 각자 관라히게 되면 코드를 두 서비스 모두 수정해야하는 단점이 있다.

프로젝트마다 다르겠지만 이 예제에서는 후자의 방법을 사용한다고 한다.

따라서 주문서비스에서 카타로그 서비스로 요청을 보내고 받을 객체를 정의해야한다.

## □ DTO 생성

```java
package com.nhn.corp.ext.orderservice.book;

public record Book (
        String isbn,
        String title,
        String author,
        Double price
){}
```

클라이언트 설정도 해준다.

```java
@Configuration
public class ClientConfig {

    @Bean
    WebClient webClient(
            ClientProperties clientProperties,
            WebClient.Builder webClientBuilder
    ){
        return webClientBuilder.baseUrl(clientProperties.catalogServiceUri().toString()).build();
    }
}
```

요청 보낼시 baseurl는 위에서 설정한 localhost:9001로 지정될 것이다.

이제 Book관련 요청을 보낼 수 있게 도와주는 BookClient 클래스를 작성한다.

```java

@Component
public class BookClient {
    private static final String BOOKS_ROOT_API = "/books/";
    private final WebClient webClient;

    public BookClient(WebClient webClient){
        this.webClient = webClient;
    }

    public Mono<Book> getBookByIsbn(String isbn) {
        return webClient
                .get()
                .uri(BOOKS_ROOT_API + isbn)
                .retrieve()
                .bodyToMono(Book.class);
    }

}
```

- retrieve(): 요청을 보내고 응답을 받는다.
- bodyToMono(): Mono형태의 Book.class를 받도록 설정즉, Mono<Book>객체를 받을 것이다.


저번장에서 OrderService에 submitOrder()메서드를 호출하면 무조건 buildRejectedOrder를 호출하여 실제 서비스간의 요청이 오고가지 않도록 하였는데, 이를 수정해야한다.

## □ Order 서비스 수정

```java
package com.nhn.corp.ext.orderservice.order.domain;

import com.nhn.corp.ext.orderservice.book.Book;
import com.nhn.corp.ext.orderservice.book.BookClient;
import org.springframework.stereotype.Service;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@Service
public class OrderService {

    private final BookClient bookClient;
    private final OrderRepository orderRepository;

    public OrderService(BookClient bookClient, OrderRepository orderRepository) {
        this.bookClient = bookClient;
        this.orderRepository = orderRepository;
    }

    public Flux<Order> getAllOrders() {
        return orderRepository.findAll();
    }

    public Mono<Order> submitOrder(String isbn, int quantity) {
        return bookClient.getBookByIsbn(isbn)
                .map(book -> buildAcceptedOrder(book,quantity))
                .defaultIfEmpty(buildRejectedOrder(isbn,quantity))
                .flatMap(orderRepository::save);
    }

    public static Order buildAcceptedOrder(Book book, int quantity) {
        return Order.of(book.isbn(), book.title() + "-" + book.author(),
                book.price(), quantity, OrderStatus.ACCEPTED);
    }

    public static Order buildRejectedOrder(String bookIsbn, int quantity) {
        return Order.of(bookIsbn, null, null, quantity, OrderStatus.REJECTED);
    }

}

```

위의 코드에서 submitOrder()는 책의 주문이 가능하면 접수를 진행하고 불가능하다면 주문을 거부하는 코드를 작성한다.

이후 주문이 가능한 Book 객체를 생성한 뒤 주문api를 통하여 테스트해본다.

```sh
> http POST :9001/books author="Jon Snow" title="" isbn="123ABC4562" price=9.98

> http POST :9002/orders isbn=123456891 quantity=3

http POST :9002/orders isbn=1234567891 quantity=3
HTTP/1.1 200 OK
Content-Length: 232
Content-Type: application/json

{
    "bookIsbn": "1234567891",
    "bookName": "Northern Lights-Lyra Silverstar",
    "bookPrice": 9.98,
    "createdDate": "2024-07-31T05:38:05.851342Z",
    "id": 1,
    "lastModifiedDate": "2024-07-31T05:38:05.851342Z",
    "quantity": 3,
    "status": "ACCEPTED",
    "version": 1
}
```

이렇게 요청이 성공하면 된다.

서비스간의 api호출은 이렇게 쉽게 성공하면 다행이지만 타임아웃이 나거나 요청이 실패하는 등, 카탈로그 서비스에서 오류를 반환하면 이를 받는 주문서비스에서는 어떻게 처리해야할까? 이에 대한 해결책을 제시해된다.

