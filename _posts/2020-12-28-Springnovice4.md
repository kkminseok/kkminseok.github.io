---
title: Spring - 3 회원 관리 예제 (1)
author: 강민석
date: 2020-12-28 00:00:00 +0800
categories: [Java,1. Spring_김영한_스프링 입문]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**


**비즈니스 요구사항의 구조는 따로 작성하지 않겠습니다. 자세한 사항은 해당 강의를 찾아주세요.**

-----

## **1. 회원 도메인과 레포지토리 만들기** ##

간단한 시나리오는 다음과 같습니다. 
- 데이터 : 회원ID, 이름
- 기능 : 회원등록, 조회
- 아직 데이터 저장소는 선정되지 않음.

#### - 회원 도메인과 리포지토리 만들기 ####

![](/assets/img/sample/Spring/C3/domain.JPG)  
경로에 들어가서 domain package와 Member java파일을 만들어줍니다.  

```java
package hello.hellospring.domain;

public class Member {

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
간단한 set(),get() 함수를 넣어줍니다. 

-----

그리고 리포지토리를 만들어주기위해

똑같은 경로에 repository 패키지를 만들어주고, MemberRepository.Interface file과 MemoryMemberRepository .java파일을 만들어줍니다.

**인터페이스 파일이란 ?** 
- 구현된 것은 아무것도 없는 기본 설계도, 추상 메서드와 상수만을 멤버로 가질 수 있는 클래스
- 인터페이스는 표준, 약속, 규칙이다.
- 제 생각에는 c++의 헤더파일을 따로 저장하는 .h파일과 비슷한 것 같습니다.  

인터페이스파일 코드  

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepositroy {
    Member save(Member member); //저장소에 회원 저장
    Optional<Member> findById(Long id); // Id찾기
    Optional<Member> findByName(String name); // name 찾기
    List<Member> findAll();//모든 값 찾기
}


```

**Optional이란?**
- findById(), findByName()함수처럼 반환값이 NULL인 경우를 대비해 만든 타입입니다.


MemoryMemberRepository파일
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import javax.swing.text.html.Option;
import java.util.*;

public class MemoryMemberRepository implements MemberRepositroy{

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;// 키값생성 멤버가 새로 저장될수록 값이 1씩 증가합니다.

    @Override
    public Member save(Member member) {
        member.setId(++sequence);// 저장할 때마다 id값을 증가시킵니다
        store.put(member.getId(),member);//member 객체에 저장된 id값과 member객체를 map객체에 넣습니다.
        return member; // 상황에따라 member를 반환합니다.
    }

    @Override
    public Optional<Member> findById(Long id) {
        //ofNullable함수는 안에 감싸고 있는 store.get()함수가 null을 반환할 경우 empty Optional 객체를 생성합니다. 
        //그리고 return을 통해 반환됩니다.
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        //.stream.filter.findAny()는 findFirst()와 달리 순서와 관계없이 먼저 찾아지는 객체를 리턴합니다.
        //.stream.filter()는 함수식 안에있는 요소드들을 새로운 스트림으로 반환하는 Stream API입니다.
        //.stream()는 stream 객체를 말합니다.
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        //ArrayList형태로 Member값들을 넣고 반환합니다.
        return new ArrayList<>(store.values());
    }
}

```

---

## **2. 테스트하기** ##

사전에 알아야할 지식 
- test폴더 안에 testFile을 만든다. File명도 해당 java파일+Tests로 작성합니다. 
ex)MemoryMemberRepositoryTests.java
- **@Test로 작성된 소스들은 클래스단위로 돌릴 시 실행순서가 정해져있지않습니다.** 그렇기 때문에 오류가 발생할 수 있습니다.

전에 작성한 MemoryMemberRepository.java 파일에
```java
 public void clearStore(){
        store.clear();
    }
```
메소드를 넣어줍니다.

![](/assets/img/sample/Spring/C3/test.JPG)  
경로에 ~Tests.java파일을 생성합니다.
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

import javax.swing.text.html.Option;

import java.util.List;

import static org.assertj.core.api.Assertions.*;

public class MemoryMemberRepositoryTest {
    //테스트를 먼저 만드는 것을 테스트 주도개발 ttd
    //테스트 객체생성
    MemoryMemberRepository repository = new MemoryMemberRepository();
    //AfterEach는 콜백함수로 각각 테스트가 끝날때 마다 호출되는 함수를 넣는다.
    @AfterEach
    public void afterEach(){
        //객체를 지워준다. 안지워주면 테스트순서에따라 객체를 잘못 조회하여 오류가 난다.
        repository.clearStore();
    }

    //save()함수 테스트 코드
    @Test
    public void save(){
        //새로운 Member 객체를 만들어준다.
        Member member = new Member();
        member.setName("Spring");

        repository.save(member);

        //result에 찾아온 값을 넣는다.
        Member result = repository.findById(member.getId()).get();//Optional이 반환값이므로
        //밑의 코드들은 전부 result값이 save하기 전 넣었던 값과 같은지 확인하는 코드들이다.
        
        //간단한 방법
        Assertions.assertEquals(member,result);
        //좀 더 간단한 방법
        assertThat(member).isEqualTo(result);
        //오류 검출하는 코드 : Assertions.assertEquals(member,null);
        // 원시적인 방법 : System.out.println("result = " + (result ==member));
    }

    @Test
    public void findByName(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member(); //refactory
        member2.setName("spring1");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();

        assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring1");
        repository.save(member2);

        List<Member> result = repository.findAll();

        assertThat(result.size()).isEqualTo(2);

    }

}
```

-----

위의 코드들은 간단한 형태의 코드들이라 부연설명은 필요한 부분만 작성하였습니다.

