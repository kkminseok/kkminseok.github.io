---
title: Spring - 6 AOP (강의 끝)
author: 강민석
date: 2021-01-03 00:00:00 +0800
categories: [Spring, kyhT&Novice&WebMVC]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**

-----

## **1. AOP가 필요한 상황** ##

AOP가 필요한 상황은 예로 들어서 모든 메소드의 호출 시간을 측정하고 싶다면(이를 공통 관심 사항 이라고 칭합니다.) 여러가지 방법이 있겠지만, 간단한 방법으로 로직안에 그대로 넣는 방법이 있을 것입니다.  

저번에 작성한 회원가입 코드 예시로
```java
 /*
    *회원가입 1. 중복 회원 고려
     */
    public Long join(Member member){

        long start = System.currentTimeMillis();

        try{
            validateDuplicateMember(member);//중복회원 검증 Method refactoring 했음.
            //밑의 함수를 간단하게 위의 메소드로 작성한 것입니다.
        /*
        Optional<Member> result = memberRepositroy.findByName(member.getName());
        result.ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });
         */
            memberRepositroy.save(member);


            return member.getId();

        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join = " + timeMs + "ms");
        }

    }
```
이 코드는 기존 코드에서 수행시간 측정 코드만 넣은 것입니다.

- 위의 상항에서는 다음과같은 문제가 있습니다.  

    - 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다.  
    - 시간을 측정하는 로직은 공통 관심 사항이다.  
    - 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.  
    - 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다.  
    - 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야 한다.  

수행시간 코드를 많은 메소드에 다 넣는다는 것은 굉장히 불편한 일입니다.

-----

## **2. AOP** ##

Aspect Oriented Programming 이라고도 합니다.

![](/assets/img/sample/Spring/C6/AOP.JPG)  
출처 : 김영한님 강의자료  

AOP를 적용하기 위해

hello.hellospring 밑에 aop라는 패키지를 만들고 TimeTraceAop라는 java파일을 만듭니다.  
![](/assets/img/sample/Spring/C6/time.JPG)  

```java
package hello.hellospring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class TimeTraceAop {

    //프록시라는 기술로 발동된다.
    //패키지 전부에 적용. 문법
    //joinPoint로 여러가지 조작이 가능하다.
    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("Start: " + joinPoint.toString());
        try{
            return joinPoint.proceed();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish-start;
            System.out.println("End: " + joinPoint.toString() + " " + timeMs +"ms");
        }
    }
}

```
이렇게 작성하면 위에서 작성한 try finally 구문을 삭제해도 모든 메서드를 시간측정을 할 수 있습니다.  
서버를 다시 키고 메소드들을 실행하면 다음과 같이 확인할 수 있습니다.
![](/assets/img/sample/Spring/C6/result.JPG)  

구조는 다음과 같이 바뀝니다.

![](/assets/img/sample/Spring/C6/aop2.JPG)  
출처 : 김영한님 강의자료  
<br>
이로써 강의가 하나 끝났습니다.  
김영한님의 강의를 통해 Spring에는 많은 기술이 있고, 해당 기술을 겉핥기식으로 배웠기에 어떤 기술인지는 알았으나,  
작동 방식 등 좀 더 자세히 공부해야겠단 생각을 했습니다.  
모든 내용을 담을 수는 없었지만, 궁금하면 해당 강의를 찾아서 볼 수 있게 작성하였습니다.


