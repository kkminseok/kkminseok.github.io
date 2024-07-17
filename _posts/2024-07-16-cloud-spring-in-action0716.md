---
title: 클라우드 네이티브 인 액션(4) - 스프링 데이터 관련
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

저번 포스팅에서는 스프링에서 다루는 설정을 알아보았다.

동적으로 설정을 추가할수도, 뺼수도 있었고 깃허브를 통해 버전관리도 할 수 있었다.

해당장에서는 애플리케이션이 종료될 때 저장해야하는 값들을 어떻게 저장할 것인가에 대한 이야기인 듯하다.

## □ Docker PostgreSQL실행

책에서는 PostgreSQL에 데이터를 저장할 것인데, 확장성 관계형 비관계형을 지원, 오픈소스 등의 장점 등을 고려하여 선택한 듯 하다.

```sh
docker run -d \
  --name polar-postgres \
  -e POSTGRES_USER=user \
  -e POSTGRES_PASSWORD=password \
  -e POSTGRES_DB=polardb_catalog \
  -p 15432:5432 \
  postgres:14.4
```

책에서는 호스트의 15432과 컨테이너의 5432의 포트를 바인딩하였지만 나는 이미 사용중인 포트라서 15432로 매핑해주었다.

도커 볼륨을 사용하지 않아서 이는 컨테이너가 종료될때마다 데이터들이 싹 날라가겠지만 아직까지는 데이터를 지속적으로 저장할 필요가 없다고 판단하였나보다.


## □ JDBC로 연결

Spring 모듈에서 PostgreSQL에 연결할 수 있도록 드라이버를 명시적으로 설정해줘야하고, 스프링 데이터 관련 라이브러리도 추가해줘야한다.

```gradle
//build.gradle
implementation 'org.springframework.boot:spring-boot-starter-data-jdbc'
//driver
runtimeOnly 'org.postgresql:postgresql'
```


이후 `properties`에 계정 정보를 입력해준다.

```properties
spring.datasource.username=user
spring.datasource.password=password
spring.datasource.url=jdbc:postgresql://localhost:15432/polardb_catalog
spring.datasource.hikari.connection-timeout=2000
spring.datasource.hikari.maximum-pool-size=5
```

데이터 베이스 연결을 열고 닫는게 리소스적 부담이 있으므로 이를 해결할 수 있어야한다.

- spring.datasource.hikari.connection-timeout: 풀에서 객체 연결을 기다리는 최대 시간
- spring.datasource.hikari.maximum-pool-size: 풀의 최대 크기

이 옵션을 통해 연결풀의 크기를 정하고, 타임아웃을 설정하여 복원력과 성능을 향상시킨다.

보통 도메인 엔티티와 지속성 엔티티 2개를 만드는데 사이즈가 크지 않으므로 기존의 `Book`레코드를 지속성 엔티티로 변경 작성할 예정이다.

지속성 엔티티는 각 객체를 식별할 수 있는 키가 있어야하며, 이 키는 코드단에서는 필드로 변환되고 DB에서는 기본키로 변환된다. 해당 필드에는 `@Id`애노테이션이 붙는다. 

동일한 엔티티에 여러 사용자가 수정작업을 거칠 때 Spring DATA JPA에서 제공하는 **낙관적 잠금**을 통하여 이를 보호할 것이다.

여기서 제공하는 낙관적 잠금은 버전을 기반으로 감지를 하는데 절차는 다음과 같다.

1. 사용자1이 업데이트를 시도 (이때의 버전 0)
2. 업데이트 되기 전 사용자2가 업데이트 시도(이때의 버전 0)
3. 사용자1의 업데이트가 완료 (이때의 버전 1)
4. 사용자2가 업데이트 완료 후 저장 하려는데 받아왔던 객체의 버전이 다름(최초 0 -> 1로 변경)

## □ 지속성 엔티티 생성 및 낙관적 잠금 적용

기존에 사용하던 Book 레코드에 @Id, @Version를 달 필드를 추가한다.

```java

public record Book(

      @Id
      Long id,

      ...
      ...

      @Version
      int version
}{
    public static Book of(String isbn, String title, String author, Double price, String publisher) {
        return new Book(null, isbn, title, author, price, publisher, null, null, 0);
    }
}
```

새로운 객체가 생길때 Id가 null이고, version이 0이면 새로운 엔티티라고 인식하게 된다.

새로운 필드가 추가되어, 관련 비즈니스 로직도 수정해줘야한다.

```java
//BookService.java
public Book editBookDetails(String isbn, Book book) {
    return bookRepository.findByIsbn(isbn)
            .map(existingBook -> {
                var bookToUpdate = new Book(
                        existingBook.id(),
                        existingBook.isbn(),
                        book.title(),
                        book.author(),
                        book.price(),
                        existingBook.version());
                return bookRepository.save(bookToUpdate);
            })
            .orElseGet(() -> addBookToCatalog(book));
}
```

엔티티 내용을 수정하면 관련 엔티티로 Id로 업데이트해주고 기존에 사용하던 version으로 변경해준다. 이때의 버전은 1이 증가된 값이 들어가게 될 것이다.

테스트용으로 사용했던 정적 팩토리 메서드도 수정해줘야한다.

```java
//BookDataLoader.java
@Component
@Profile("testdata")
public class BookDataLoader {
    private final BookRepository bookRepository;

    public BookDataLoader(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    @EventListener(ApplicationReadyEvent.class)
    public void loadData() {
        var book1 = Book.of("1234567891", "Test", "Lyra", 9.91);
        var book2 = Book.of("1234567892", "Test book", "Polar", 9.94);
        bookRepository.save(book1);
        bookRepository.save(book2);
    }
}
```

인자로 수정된 필드값을 넣어주지 않았는데, 프레임워크단에서 알아서 Id,version을 매핑해주기 때문이다.

이렇게 실행하면 테스트코드에서 문제가 생길텐데, 이 또한 위와같이 수정해주면 된다.

스프링 데이터에는 애플리케이션이 시작될 때 관련 테이블을 생성할 수 있게 도와주는 기능을 제공한다.

## □ 데이터베이스 스키마 생성

기본적으로 `src/main/resources`에 존재하는 `schema.sql`을 통해 스키마를 생성할 수 있게 한다. 

해당 경로에 파일을 작성하면 된다.

```sql
-- /src/main/resources/schema.sql
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

당연히 운영환경에서는 사용하기 애매한 감이 있고, 인메모리 데이터베이스를 사용할 경우에만 자동으로 해당 구문을 적용하므로 따로 설정을 해줘서 해당 sql을 실행하게끔 해야한다.

```properties
spring.sql.init.mode=always
```

이를 통해서 애플리케이션이 시작할때마다 관련 테이블을 삭제했다가 뜰 수 있게 한다.

## □ JDBC 감사 활성화

테이블의 각 행에 대해서 수정날짜 및 생성날짜를 알면 서비스를 유지보수, 관리하는데에 이점이 많다. Spring Data는 개발자가 수동으로 관련 코드를 삽입하지 않게끔 기능을 제공하고 있다.

관련 설정 클래스 파일 생성하여 빈에 등록해주고 `@@EnableJdbcAuditing` 애노테이션을 달아주면 해당 클래스 파일을 통해서 데이터 감사 기능을 추가할 수 있다.

```java
package com.polarbookshop.catalogservice.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.jdbc.repository.config.EnableJdbcAuditing;

@Configuration
@EnableJdbcAuditing
public class DataConfig {
}
```

이를통해 데이터가 변경,삭제 등의 작용이 일어날때마다 감사 이벤트가 생성된다.
스프링 데이터는 이 감사 이벤트에 대한 정보를 받아 데이터베이스에 저장할 수 있게 도와주는 애노테이션을 제공한다.

대표적으로는 `@CreatedDate`, `@LastModifiedDate`가 있는데 이를 Book 레코드에 달아주면 된다.

```java
public record Book(
    ...
    @CreatedDate
    Instant createdDate,

    @LastModifiedDate
    Instant lastModifiedDate
    ...
)
```





