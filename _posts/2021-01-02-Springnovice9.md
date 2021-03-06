---
title: Spring - 5 스프링 DB접근 기술 (2)
author: 강민석
date: 2021-01-02 12:00:00 +0800
categories: [Spring, kyhT&Novice&WebMVC]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**

-----

## **1. Spring JDBC template** ##

JdbcTemplate이란 JDBC API에서 본 'connect()'와 같은 반복 코드를 줄여주는 역할을 햊부니다. 하지만 SQL은 직접 작성해야합니다.  
SQL은 뒤에 작성할 JPA를 통해 줄여나갈 수 있다고 합니다.

repository package에서 JdbcTemplateMemberRespository라는 자바파일을 만들어줍니다.

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;

import javax.sql.DataSource;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Optional;

public class JdbcTemplateMemberRepository implements MemberRepository{

    private final JdbcTemplate jdbcTemaplate;

    //spring이 자동으로 datsoruce를 injection 해줌.
    @Autowired
    public JdbcTemplateMemberRepository(DataSource dataSource){
        jdbcTemaplate = new JdbcTemplate(dataSource);
    }

    //감 잡기용.
    @Override
    public Member save(Member member) {
        SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemaplate);
        jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

        Map<String, Object> parameters = new HashMap<>();
        parameters.put("name", member.getName());

        Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
        member.setId(key.longValue());
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        List<Member> result = jdbcTemaplate.query("select * from member where id = ?",memberRowMapper(),id);
        return result.stream().findAny();
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = jdbcTemaplate.query("select * from member where name = ?", memberRowMapper(), name);
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return jdbcTemaplate.query("select * from member",memberRowMapper());
    }

    private RowMapper<Member> memberRowMapper(){
        return (rs, rowNum) -> {

            Member member = new Member();
            member.setId(rs.getLong("id"));;
            member.setName(rs.getString("name"));;
            return member;
        };
    }
}

```

위와같이 코드를 작성해준 다음 전에 작성했던 SpringConfig에서 스프링이 알 수 있게 다음과 같이 코드를 작성합니다.

```java
 @Bean
    public MemberRepository memberRepositroy(){
        //return new MemoryMemberRepository();
        //return new JdbcMemberRepository(dataSoruce);
        return new JdbcTemplateMemberRepository(dataSoruce);
    }
```
테스트를 실행해서 오류가 뜨지 않으면 성공입니다.

-----

## **2. JPA** ##

SQL쿼리도 JPA가 자동으로 처리하여 개발 생산성을 크게 늘려주는 기술 --> 객체 중심의 설계로 패러다임을 전환할 수 있다고 합니다.  

마이바티스에 비해 JPA는 해외적으로 비교도 안 될 정도로 많이 사용하고 있다.  

먼저 jpa,h2 데이터베이스 관련 라이브러리를 추가하기 위해
build.gradle의 코드를 바꿔줍니다.
```java
plugins {
	id 'org.springframework.boot' version '2.4.1'
	id 'io.spring.dependency-management' version '1.0.10.RELEASE'
	id 'java'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	//implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	//jdbc,jpa 포함
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	runtimeOnly 'com.h2database:h2'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

test {
	useJUnitPlatform()
}

```
그 후, apllication.properties에서
```console
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa

spring.jpa.show-sql=true 
spring.jpa.hibernate.ddl-auto=none
```
로 밑의 2줄을 추가해줍니다.  
spring.jpa.show-sql=true 는 jpa가 생성하는 SQL을 출력합니다.  
spring.jpa.hibernate.ddl-auto=none은 jpa가 자동으로 테이블을 생성하는 기능을 none을 통해 해당 기능을 끕니다. 왜냐하면 이미 만들어진 테이블을 갖고 테스트를 진행하기 때문입니다.


예전에 작성했던 domain/Member.java 파일을 수정해줘야합니다.
```java
package hello.hellospring.domain;

import javax.persistence.*;

//jpa가 관리하는 entity가 되는 것.
@Entity
public class Member {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;//시스템이 정하는 id
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
그리고 repository pakage에서 JpaMemberRepository.java를 만들어주고 다음 코드를 넣습니다.
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.hibernate.type.EntityType;
import org.springframework.transaction.annotation.Transactional;

import javax.persistence.EntityManager;
import javax.swing.text.html.Option;
import java.util.List;
import java.util.Optional;


public class JpaMemberRepository implements MemberRepository{

    //주입받아야한다.
    private final EntityManager em;

    public JpaMemberRepository(EntityManager em){
        this.em = em;
    }

    @Override
    public Member save(Member member) {
        //영속적으로 저장한다. jpa가 insert query만들어서 db에 집어넣는다.
        //혁식적
        em.persist(member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        //조회할 타입이랑 식별자를 넣어줌.
        Member member = em.find(Member.class,id);
        return Optional.ofNullable(member);
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name",name)
                .getResultList();

        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        //멤버 entity 대상으로 쿼리를 보냄.
        //select문을 보면 m entity 객체 자체를 select함.
        return em.createQuery("select m from Member m", Member.class).getResultList();

    }
}

```

마지막으로 MemberSerivce.java파일인 서비스계층에 트랜잭션을 추가해줘야합니다.
```java
@Transactional
public class MemberService {~~
}
```
@Transactional을 추가해줍니다.  
- 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을 커밋합니다. 만약 런타임 예외가 발생하면 롤백합니다.  
- **JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행되어야 합니다.**  

마지막으로 JPA를 사용하도록 스프링 설정을 변경해야합니다.  
SpringConfig.java파일에서
```java
package hello.hellospring.service;

import hello.hellospring.repository.JdbcMemberRepository;
import hello.hellospring.repository.JdbcTemplateMemberRepository;
import hello.hellospring.repository.JpaMemberRepository;
import hello.hellospring.repository.MemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.sql.DataSource;

@Configuration
public class SpringConfig {

    private EntityManager em;

    public SpringConfig(EntityManager em){
        this.em = em;
    }

    /*
    private final DataSource dataSoruce;

    @Autowired
    public SpringConfig(DataSource dataSource){
        this.dataSoruce = dataSource;
    }
     */
    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepositroy());
    }

    @Bean
    public MemberRepository memberRepositroy(){
        //return new MemoryMemberRepository();
        //return new JdbcMemberRepository(dataSoruce);
        //return new JdbcTemplateMemberRepository(dataSoruce);
        return new JpaMemberRepository(em);
    }
}

```
이처럼 Jpa를 사용할 수 있게 객체 생성, Bean에 담아줍니다.
테스트를 해보면
![](/assets/img/sample/Spring/C5/result2.JPG)  
처럼 insert ~ 문구가 뜨면 테스트가 진행된 것이고 

만약 값이 들어가는 지 눈으로 확인하고 싶다면 전에 작성한 MemberServiceIntegrationTest.java 파일에서 회원가입()위에
```java
~~~
    //중복 에러가 뜨면 DB를 비워주자. test용 DB를 구축
    @Test
    //DB에 반영하도록 함.
    //@Commit
    void 회원가입() {
        //given 무언가 주어졌을 때
        Member member = new Member();
        member.setName("spring");
        //when 어떤 상황에서
        Long saveId = memberService.join(member);
        //then 무언가 나와야한다.
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }
~~~
```
@Commit이란 것을 달아주면 DB에 직접 반영됩니다. 하지만 나중에 데이터를 지워줘야합니다.
