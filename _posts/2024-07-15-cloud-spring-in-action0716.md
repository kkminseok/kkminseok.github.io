---
title: 클라우드 네이티브 인 액션(5) - 컨테이너화
author: minseok
date: 2024-07-15 00:02:00 +0800
categories: [Spring]
tags: [Spring]
math: true
mermaid: true
image: 
    path: /assets/img/cloudNativeSpringInAction/main.jpeg
    alt: Cloud Native Spring In Action
comments: true
---

> 클라우드 네이티브 스프링 인 액션 서적의 데모 프로젝트를 모방하였습니다.
[깃 레포지토리](https://github.com/kkminseok/spring-cloud-native-example)
{: .prompt-info}

해당 장에서는 작성한 애플리케이션을 컨테이너화하여 실행하는 방법에 대해 배운다.

## □ 스프링 애플리케이션 도커 컨테이너화

도커 네트워크를 통해 postgresql 컨테이너와 Spring boot Application을 연결해야하므로 도커 네트워크를 만들어주자.

```sh
> docker network create catalog-network
fde43c98c153f3dd108e8663baca1c9a0e708d5a32531ab56491a1738576db66
```

이후, postgresql을 도커네트워크에 연결해서 다시 띄우도록 한다.

```sh
docker run -d \
  --name polar-postgres \
  --net catalog-network \
  -e POSTGRES_USER=user \
  -e POSTGRES_PASSWORD=password \
  -e POSTGRES_DB=polardb_catalog \
  -p 15432:5432 \
  postgres:latest
```

이후 스프링 프로젝트 루트 디렉터리에 터미널을 열어서 도커파일을 작성해준다.

```docker
FROM eclipse-temurin:17
WORKDIR workspace
LABEL authors="kms"

ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} catalog-service.jar
ENTRYPOINT ["java", "-jar", "catalog-service.jar"]
```

jre을 미리제공하는 이미지를 베이스로 하였으며, workspace가 실제 스프링부트가 띄워지는 디렉터리가 될 것이다.

jar파일은 현재상태에서 미리 제공되어야하므로, build를 실행해준다.

```sh
 ./gradlew clean bootJar
```

이후 컨테이너 이미지를 만들어 준다.

```sh
docker build -t catalog-service .
```

버전을 지정하지 않아서 태그가 latest로 생성될 것이다.





