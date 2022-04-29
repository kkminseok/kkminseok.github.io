---
title: Spring - 4 스프링 빈과 의존관계
author: 강민석
date: 2020-12-30 01:00:00 +0800
categories: [Java,1. Spring_김영한_스프링 입문]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**

-----

## **1. 회원 컨트롤러에 의존관계 추가** ##

회원 컨트롤러가 회원서비스와 회원 리포지토리를 사용할 수 있게 의존관계를 준비하겠습니다.  
방식은 2가지가 있는데, 먼저 첫 번째 방법인 컴포넌트 스캔 방법입니다.

MemberController에 들어가 다음 코드를 추가해줍니다.

```java
@Controller// controller라는 것을 알림.
public class MemberController {

    private final MemberService memberService;

    //스프링 컨테이너에서 멤버 서비스를 가져옴. Dependency injection = DI 의존성 주입.
    @Autowired
    public MemberController(MemberService memberService){
        this.memberService = memberService;
    }
}
```

코드를 실행해보면 오류가 납니다.  
오류내용을 보면 memberService가 스프링 빈으로 등록되어 있지 않아서 나타나는 오류입니다. 참고로 helloController는 스프링이 제공하는 컨트롤러여서 스프링 빈으로 자동 등록됩니다.(@Controller가 있어야함.)  

- @Component 애노테이션이 있으면 스프링 빈으로 자동 등록됩니다.
- @Controller 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문입니다.
- @Component를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록됩니다.
- @Controller, @Service, @Repository 에 들어가면 @Component를 가지고 있습니다.  

![](/assets/img/sample/Spring/C4/controller.JPG)
![](/assets/img/sample/Spring/C4/service.JPG)
![](/assets/img/sample/Spring/C4/repo.JPG)

Service와 Repository를 등록하기 위해  MemberRepository에
```java
@Service
public class MemberService {
 private final MemberRepository memberRepository;
 @Autowired
 public MemberService(MemberRepository memberRepository) {
 this.memberRepository = memberRepository;
 }
}
```

repository를 등록하기 위해 MemoryMemberRepository

```java
@Repository
public class MemoryMemberRepository implements MemberRepository {}
```

스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록합니다. 따라서 같은 스프링 빈으로 모두 같은 인스턴스입니다. 

-----

**두번째 방법은 자바코드로 직접 스프링 빈에 등록하는 방법입니다.**

위에서 작성한 @Service, @Repository, @Autowired 애노테이션을 제거하고 진행합니다.

Service 패키지에 SpringConfig라는 java파일을 만들어줍니다.  
![](/assets/img/sample/Spring/C4/sc.JPG)  

```java
package hello.hellospring.service;

import hello.hellospring.repository.MemberRepositroy;
import hello.hellospring.repository.MemoryMemberRepository;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepositroy());
    }

    @Bean
    public MemberRepositroy memberRepositroy(){
        return new MemoryMemberRepository();
    }
}
```
DI에는 필드주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있습니다. 각각 자세한 내용은 여기를 참고하였습니다.  
<https://yaboong.github.io/spring/2019/08/29/why-field-injection-is-bad/>

결론은 생성자 주입 사용을 권장한다는 것입니다.

