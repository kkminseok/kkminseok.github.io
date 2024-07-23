---
title: 클라우드 네이티브 인 액션(4) - 스프링 데이터 관련
author: minseok
date: 2024-07-15 00:02:00 +0800
categories: [Spring]
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

인터페이스로 제작하는 이유는 비즈니스 로직이 데이터에 접근만 하면 되고, 어디서 왔는지는 알 필요는 없기 때문이다.

실습에 사용했던 `InMemoryBookRepository` 클래스가 있다면 해당 클래스는 삭제하고, `BookRepository` 인터페이스를 만든다.

스프링 데이터는 데이터 소스에 대한 레포지토리 구현을 이미 갖고 있고, 현재는 CRUD를 작성해야하므로 `CrudRepository`를 확장받을 수 있도록 한다.

```java
public interface BookRepository extends CrudRepository<Book, Long> {

    Optional<Book> findByIsbn(String isbn);

    Boolean existsByIsbn(String isbn);

    @Modifying
    @Transactional
    @Query("delete FROM book where isbn= :isbn")
    void deleteByIsbn(String isbn);
}
```

@Id애노테이션이 달린 값을 기준으로 CRUD를 수행하는데, 해당 예제에서는 @Id애노테이션이 달리지 않은 isbn으로 삭제작업을 진행할 것이므로 `@Query`구문을 이용하여 따로 jpql을 작성한다.

`@Modify`는 이 데이터가 수정됨을 명시적으로 알려주는 기능 외에도, 트랜잭션 처리를 알리는 기능, 해당 메서드를 호출하기전에 변경사항을 플러시하게끔 하는 기능 등을 제공한다.

`@Modify`는 보통 `@Transcational`과 같이 쓰이므로 해당 애노테이션도 달아준다.




스프링 데이터에서 제공하는 메서드(deleteAll, saveAll)을 이용해서 정적 팩토리 메서드도 수정해준다.

```java
        bookRepository.deleteAll(); // <--
        var book1 = Book.of("1234567891", "Test", "Lyra", 9.91, "kms");
        var book2 = Book.of("1234567892", "Test book", "Polar", 9.94, "kms");
//        bookRepository.save(book1);
//        bookRepository.save(book2);
        bookRepository.saveAll(List.of(book1,book2)); // <--
```

## □ 테스트 컨테이너 등으로 테스트 작성

테스트 컨테이너라는 지원서비스를 통해 통합테스트를 진행해볼 예정이다.

테스트 컨테이너를 사용하기 위해서 의존성을 추가해준다.

```gradle
ext{ 
    ...
    set('testcontainersVersion',"1.19.8")
}

dependencies{
    ...
    testImplementation 'org.testcontainers:postgresql'
    ...
}

dependencyManagement{
    imports {
        ...
        mavenBom "org.testcontainers:testcontainers-bom:${testcontainersVersion}"
    }
}
```

이후 `/src/test/resoucres`에 application-integration.yml파일을 작성한다.

해당 프로파일을 활성화해서 기본속성의 datasource.url을 덮어씌울 것이다.

따라서 파일이름은 굳이 applciation-integration이 아니여도 된다.

```yml
spring:
  datasource:
    url: jdbc:tc:postgresql:16.3:///polardb_catalog
```

### @DataJdbcTest를 이용한 슬라이스 테스트 작성


```java

@DataJdbcTest
@Import(DataConfig.class)
@AutoConfigureTestDatabase(
        replace = AutoConfigureTestDatabase.Replace.NONE
)
@ActiveProfiles("integration")
class BookRepositoryJdbcTests {

    @Autowired
    private BookRepository bookRepository;

    @Autowired
    private JdbcAggregateTemplate jdbcAggregateTemplate;

    @Test
    void findBookByIsbnWhenExisting() {
        var bookIsbn = "1234561237";
        var book = Book.of(bookIsbn, "title","author",3.3, "kms");
        jdbcAggregateTemplate.insert(book);
        Optional<Book> actualBook = bookRepository.findByIsbn(bookIsbn);
        assertThat(actualBook).isPresent();
        assertThat(actualBook.get().isbn()).isEqualTo(book.isbn());


    }

}
```

- @DataJbcTest: Spring Data Jdbc, JdbcTemplate 관련 컴포넌트만 로드하여 빠르고 가벼운 테스트를 지원한다. 또한 각 메서드를 트랜잭션화하여 해당 메서드가 종료되면 롤백을 시킨다.
- @Import(Dataconfig.class): 감사 설정을 위해 추가한 애노테이션
- @AutoConfigureTestDatabase(
        replace = AutoConfigureTestDatabase.Replace.NONE
): 테스트용 컨테이너 데이터베이스를 사용하기에 내장 데이터베이스 사용을 하지 않는다는 뜻
- @ActiveProfiles("integration"): 해당 프로파일을 활성화하여 테스트용 컨테이너 접속정보를 공유


### @SpringBootTest를 사용한 통합 테스트 작성

앞서 통합테스트를 작성한 적이 있는데, 프로파일을 활성화하여 테스트컨테이너를 적용시키도록 한다.

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@ActiveProfiles("integration")
class CatalogServiceApplicationTests {

}
```

## □ 플라이웨이를 통한 데이터베이스 관리

플라이웨이라는 기술은 처음들어본다.

간단히 이이갸히자면 데이터베이스용 git같은 느낌이다.
데이터베이스용 형산관리, 버전관리 도구이다.

플라이웨이는 **flyway_schema_history**테이블을 통해 데이터베이스의 버전을 관리한다.

플라이웨이는 독립적으로 사용이 가능하나 스프링 부트가 자동설정을 지원하므로 애플리케이션이랑 같이 사용할 수도 있다. 이런경우에는 src/main/db/migration에서 sql 마이그레이션 및 자바 마이그레이션을 검색한다.

확인하기 위해서 의존성을 추가한다.

```gradle
implementation 'org.flywaydb:flyway-core'
```

플라이웨이는 데이터 초기화 기능을 제공하므로 전에 작성했던 `schema.sql`을 삭제하고, `application.properties`에서 `spring.sql.init.mode=always` 속성을 제거해준다.

그리고 /src/main/resources/db/migration에서 `V1__Initial_schema.sql`를 작성해준다.

```sql
CREATE TABLE book (
                      id                  BIGSERIAL PRIMARY KEY NOT NULL,
                      author              varchar(255) NOT NULL,
                      isbn                varchar(255) UNIQUE NOT NULL,
                      price               float8 NOT NULL,
                      title               varchar(255) NOT NULL,
                      created_date        timestamp NOT NULL,
                      last_modified_date  timestamp NOT NULL,
                      version             integer NOT NULL
);
```

파일명은 규칙이 있다.

- 접두사: 버전 마이그레이션은 V를 사용
- 버전: 점이나 밑줄로 버전정의
- 구분자: **두개의 밑줄(__)**
- 설명: 밑줄한개로 구분되는 하나 이상의 단어
- 접미사: sql

해당 설정을 하고 PostgreSQL 컨테이너를 띄운상태에 확인해보면 스키마 초기화가 잘 이뤄졌음을 알 수 있다.


여기서 만약 고객사의 요청에 따라 컬럼을 하나(publisher) 추가하고 해당 컬럼에 대한 정보를 제공해야한다고 할 때 어떻게 해야하는가?

위의 명명규칙에 따라 `V2__Add_publisher_column.sql`를 작성한다.

```sql
ALTER TABLE book
ADD COLUMN publisher varchar(255);
```

자바레코드와 비즈니스 로직도 손봐줘야하는데, 이미 배포된 상황에서 이전 값들에 대한 유효성 검사를 하지 않기 위해 필수값이 아닌 선택적 값으로 설정한다.

```java
public record Book(
    ...
    String publisher,
    ...
){
    public static Book of(String isbn, String title, String author, Double price, String publisher) {
        return new Book(null, isbn, title, author, price, publisher, null, null, 0);
    }
}
```

이후 프러덕션 환경에 배포를 진행하면 플라이웨이는 V1~ sql은 이미 적용되었기에 V2__Add_publisher_column.sql를 수행하게 된다.

프러덕션 환경에서 배포할 때 주의점을 책에서 알려주었다.

다운타임을 없애기 위해 롤링 업데이트 등으로 이전버전과 새 버전의 애플리케이션이 동시에 떠있을텐데, 이 때 이전버전에서 문제가 생기지 않게끔 수정해야한다는 것이다.

만약 추가한 컬럼이 필수값이라면 이전 버전에서는 문제가 발생할 것이다.

이 책 뒤에서는 이런경우에도 대처할 수 있는 방안을 알려주는 것 같다. 더 읽어봐야겠다.

추가로 플라이웨이가 이렇게만 봤을때 불편한 점이 많아보이는데, 실무에서 어떻게 쓰이는지 좀 더 확인해봐야겠다는 생각을했다.

결과적으로는 현재 내가 있는 실무에서 쓰이긴 어렵다고 판단하였다.

1. 이미 테이블 수정시에 테이블히스토리에 저장하는 로직이 따로 존재함.
2. 버전관리를 치밀하게 할 만큼 개발인력이 많지가 않고, 자주 수정되지 않음.

때문에 현재 내가 일하고 있는 직장에서는 쓰이기가 어려울 것 같다