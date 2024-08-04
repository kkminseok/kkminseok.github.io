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
- 
