---
title: 클라우드 네이티브 인 액션(5) - 컨테이너화
author: minseok
date: 2024-07-16 00:02:00 +0800
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

이후 이미지를 컨테이너로 띄워준다.

```sh
docker run -d \
--name catalog-service \
--net catalog-network \
-p 9001:9001 \
-e SPRING_DATASOURCE_URL=jdbc:postgresql://polar-postgres:5432/polardb_catalog \
-e SPRING_PROFILES_ACTIVE=testdata \
catalog-service
```

네트워크를 DB와 공유해야하기에 환경변수로 데이터베이스 url을 덮어 씌워주고 testdata프로파일을 활성화하여 테스트 데이터를 넣도록 한다.

이후 9001포트에 GET요청을 보내 테스트를 해보면

```sh
> http :9001/books
HTTP/1.1 200
Connection: keep-alive
Content-Type: application/json
Date: Thu, 18 Jul 2024 12:54:21 GMT
Keep-Alive: timeout=15
Transfer-Encoding: chunked

[
    {
        "author": "Lyra",
        "createdDate": "2024-07-18T12:54:15.781522Z",
        "id": 3,
        "isbn": "1234567891",
        "lastModifiedDate": "2024-07-18T12:54:15.781522Z",
        "price": 9.91,
        "publisher": "kms",
        "title": "Test",
        "version": 1
    },
    {
        "author": "Polar",
        "createdDate": "2024-07-18T12:54:15.787297Z",
        "id": 4,
        "isbn": "1234567892",
        "lastModifiedDate": "2024-07-18T12:54:15.787297Z",
        "price": 9.94,
        "publisher": "kms",
        "title": "Test book",
        "version": 1
    }
]

```

아주 잘 호출되는걸 볼 수 있다.

하지만 매 번 도커 CLI를 통해서 컨테이너를 띄우는건 휴먼 이슈도 있을것이고 불편하다.

도커 컴포즈를 향후 사용할 것이지만 그 전에 프러덕션 환경을 위한 컨테이너 이미지 빌드라고, 스프링 부트에서 지원하는 **계층화된 JAR**을 사용해서 이미지를 보다 효율적으로 제작할 수 있는 기능이 있다.

계층화된 JAR가 등장한 이유는 간단하다.

성능이다.

현재는 빌드된 파일을 이미지 레이어에 추가하여 빌드를 진행하고 있는데, 예를 들어서 컨트롤러에서 코드 하나만 수정해도 전체 빌드를 다시해야하는 불편함이 있다.

계층화된 JAR는 도커 이미지와 비슷하게 여러 레이어로 만들어지는데, 구조는 다음과 같다.

- 의존성 계층: 프로젝트에 추가된 모든 주요 의존성
- 스프링 부트 로더: 스프링 부트 로더 컴포넌트가 사용하는 클래스
- 스냅숏 의존성 계층: 모든 스냅숏 의존성
- 애플리케이션 계층: 애플리케이션 클래스 및 리소스

이렇게 이루어져있는데, 위의 예시와 같이 컨트롤러만 수정되는 경우는 애플리케이션 계층만 작성하면 된다.

이 전략을 사용하기 위해서 일단 Dockerfile을 수정한다.

## □ 계층화된 JAR Dockerfile작성

```docker
FROM eclipse-temurin:17 AS builder
WORKDIR workspace
LABEL authors="kms"

ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} catalog-service.jar
RUN java -Djarmode=layertools -jar catalog-service.jar extract

FROM eclipse-temurin:17
RUN useradd spring
USER spring
WORKDIR workspace
COPY --from=builder workspace/dependencies/ ./
COPY --from=builder workspace/spring-boot-loader/ ./
COPY --from=builder workspace/snapshot-dependencies/ ./
COPY --from=builder workspace/application/ ./

ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
```

JAR파일에서 계층을 추출하고, 각 계층을 별도의 이미지 레이어로 배치한다.  이로인해서 최종 컨테이너 이미지가 생성된다.

여기서 추가된 다른 부분은 보안에 관련된 부분인데, docker 컨테이너는 루트 사용자로 실행되기에 최소권한의 원칙에 따라 필요 이상의 권한을 갖지 않은 사용자를 만들어서 해당 사용자가 도커파일에 정의된 엔트리포인트를 실행하게끔 했다.

해당 도커파일을 기준으로 빌드를 실행해준다.

```sh
 > docker build -t catalog-service .
[+] Building 3.5s (15/15) FINISHED                                                                                                                                                         docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                       0.0s
 => => transferring dockerfile: 624B                                                                                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/eclipse-temurin:17                                                                                                                                      2.7s
 => [internal] load .dockerignore                                                                                                                                                                          0.0s
 => => transferring context: 2B                                                                                                                                                                            0.0s
 => [internal] load build context                                                                                                                                                                          0.0s
 => => transferring context: 235B                                                                                                                                                                          0.0s
 => CACHED [stage-1 1/7] FROM docker.io/library/eclipse-temurin:17@sha256:39fc8112490b67760d2dd3c8d73176f0ed323e99e7a7f09449d44fb8d1b350cc                                                                 0.0s
 => [stage-1 2/7] RUN useradd spring                                                                                                                                                                       0.2s
 => CACHED [builder 2/4] WORKDIR workspace                                                                                                                                                                 0.0s
 => CACHED [builder 3/4] COPY build/libs/*.jar catalog-service.jar                                                                                                                                         0.0s
 => [builder 4/4] RUN java -Djarmode=layertools -jar catalog-service.jar extract                                                                                                                           0.6s
 => [stage-1 3/7] WORKDIR workspace                                                                                                                                                                        0.0s
 => [stage-1 4/7] COPY --from=builder workspace/dependencies/ ./                                                                                                                                           0.1s
 => [stage-1 5/7] COPY --from=builder workspace/spring-boot-loader/ ./                                                                                                                                     0.0s
 => [stage-1 6/7] COPY --from=builder workspace/snapshot-dependencies/ ./                                                                                                                                  0.0s
 => [stage-1 7/7] COPY --from=builder workspace/application/ ./                                                                                                                                            0.0s
 => exporting to image                                                                                                                                                                                     0.1s
 => => exporting layers                                                                                                                                                                                    0.1s
 => => writing image sha256:9e4bff175df64e28ecd58accc378f1de7dc20908efb5ac2857a9afb19584976d                                                                                                               0.0s
 => => naming to docker.io/library/catalog-service                                                                                                                                                         0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/l5b5n2419jqzgb1cvoisx3wta

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview 

```

참고로 앞에서 이야기한 `grype`를 가지고 취약점 검사도 할 수 있다.

`brew install grype`로 grype를 설치하여 취약성 검사를 실행해본다.

```sh
➜  catalog-service git:(main) ✗ grype catalog-service
 ⠋ Vulnerability DB                ━━━━━━━━━━━━━━━━━━━━  [79 MB / 176 MB]  
 ✔ Loaded image                                                                                                                                                                         catalog-service:latest
 ✔ Parsed image                                                                                                                        sha256:9e4bff175df64e28ecd58accc378f1de7dc20908efb5ac2857a9afb19584976d
 ✔ Cataloged contents                                                                                                                         dbf89ba6aad1148a79f7889166105cdac426497571799ae4227742c542a72247
   ├── ✔ Packages                        [199 packages]  
   ├── ✔ File digests                    [4,599 files]  
   ├── ✔ File metadata                   [4,599 locations]  
   └── ✔ Executables                     [856 executables]  

```

- Instant 클래스는 좀 더 살펴보면 좋겠지만 간단히 얘기하자면 사람이 읽기엔 불편하지만 연산이 편리하고 서버를 외국에 둘 경우(?) 해당 로컬 시간을 따라가지 않는다는 장점이 있다.

마찬가지로 비즈니스 로직도 수정해줘야한다.

```java
    public Book editBookDetails(String isbn, Book book) {
        return bookRepository.findByIsbn(isbn)
                .map(existingBook -> {
                    var bookToUpdate = new Book(
                            existingBook.id(),
                            existingBook.isbn(),
                            book.title(),
                            book.author(),
                            book.price(),
                            existingBook.createdDate(), // <--
                            existingBook.lastModifiedDate(), // <--
                            existingBook.version());
                    return bookRepository.save(bookToUpdate);
                })
                .orElseGet(() -> addBookToCatalog(book));
    }
```

sql도 수정해줘야한다.

```sql
DROP TABLE IF EXISTS book;
CREATE TABLE book(
    id BIGSERIAL PRIMARY KEY NOT NULL,
    author varchar(255) NOT NULL,
    isbn varchar(255) UNIQUE NOT NULL,
    price float8 NOT NULL,
    title varchar(255) NOT NULL,
    created_date timestamp NOT NULL,
    last_modified_date timestamp NOT NULL,
    version integer NOT NULL
)
```

## □ 데이터베이스 레포지토리 생성

데이터 레포지토리 추상화하여 인터페이스를 제작한다.





