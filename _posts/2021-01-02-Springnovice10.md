---
title: Spring - 5 스프링 DB접근 기술 (3)
author: 강민석
date: 2021-01-02 15:00:00 +0800
categories: [Spring, kyhT&Novice&WebMVC]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**

-----

## **1. 스프링 데이터 JPA** ##

인터페이스만으로 개발을 완료할 수 있게 도와줌. 반복 개발해온 CRUD 기능도 스프링 데이터 JPA가 모두 제공합니다.  
반복되는 개발 코드들이 확연하게 줄여든다는 것입니다.  
- JPA에 대한 선행 학습이 있어야 이해하는데 도움이 된다고 합니다.JPA를 도와주는 기술이여서 JPA에 대한 선행이 필요하다고 생각하기 때문입니다.  
<br>
<br>

repository 패키지에서 SpringDataJpaMemberRepository라는 Interfacefile을 만들어줍니다.. 이름이 너무 긴 감이 있지만..
다음 코드를 작성해줍니다.

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.data.jpa.repository.JpaRepository;

import javax.swing.text.html.Option;
import java.util.Optional;

public interface SpringDataJpaMemberRepository extends JpaRepository<Member,Long>, MemberRepository{

    //구현체를 자동으로 만들어주고 Spring Bean에 등록함. 가져다 쓰기만하면 된다.
    @Override
    Optional<Member> findByName(String name);

}
```

그 다음, SpringConfig파일도 Spring이 인식할 수 있게 수정해줘야합니다.

```java
package hello.hellospring.service;
import hello.hellospring.repository.*;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
@Configuration
public class SpringConfig {
    private final MemberRepository memberRepository;

    public SpringConfig(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository);
    }
}
```

테스트를 하고 아무일 없다면 성공

-----

- 아직 초보인 제 생각에는 JPA 데이터 Spring이란, c++로 치면 STL과 같이 사람들이 많이 쓰는 함수들을 미리 작성해놓은 코드들을 말하는 것 같습니다.  
- 다양한 함수들이 있고, 해당 함수들을 가져다 쓰면 되는 것 같습니다.

![](/assets/img/sample/Spring/C5/springdata.JPG)