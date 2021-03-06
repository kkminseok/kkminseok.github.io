---
title: Spring - 3 스프링 핵심 원리 - 객체 지향 원리 적용(2)
author: 강민석
date: 2021-01-19 10:00:00 +0800
categories: [Spring,KYHT&Novice&Point&Basic]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **6. 정리** ##

- SRP 단일 책임 원칙
    + 구현 객체를 생성하고 연결하는 책임은 Appconfing이 담당.
    + 클라이언트 객체는 실행하는 책임만 담당.
    + 따라서 SRP 단일 책임 원칙을 따름.
- DIP 의존관계 역전 원칙
    + 먼저 클라이언트가 추상화에 의존하도록 수정함.
    + 이것만으로는 실행이 되지 않기에 Appconfig을 통해 의존관계를 주입하였다.
    + DIP 원칙을 따름
- OCP
    + 어플리케이션의 사용영역과 구성영역(Appconfig)으로 나눔.
    + Appconfig가 의존관계를 'FixDiscountPolicy'에서 'RateDiscountPolicy'로 변경해서 클라이언트 코드에 주입하므로 클라이언트 코드는 변경하지 않아도 된다.
    + 소프트웨어 요소를 확장해도 사용영역 변경에는 닫혀있으므로 OCP 원칙을 따름.

-----  

## **7. IOC,DI, 컨테이너** ##

- IOC(제어의 역전)
    + 기존 코드는 클라이언트 구현 객체가 서버 구현 객체를 생성하여, 연결하고 실행했다. 한마디로 클라이언트 구현 객체가 프로그램 제어권을 가직 있었다.
    + 반면 Appconfig을 만든 이후로는 제어권을 Appconfig이 가져갔다. 심지어 Impl 클래스들도 Appconfig이 생성한다. 또한 굳이 Impl 클래스들이 아니여도 Appconfig이 다른 클래스들의 객체를 생성하고 실행할 수 있다. Impl 클래스들은 그런 사실도 모른채 수행될 뿐이다.
    + 이렇듯 프로그램의 제어의 흐름을 직접 제어하는게 아니고 외부에서 제어하는 것을 IOC(제어의 역전)이라고 부른다.

- 프레임 워크 vs 라이브러리
    + 프레임 워크 : 내가 작성한 코드를 제어하고, 대신 실행하면 프레임워크(ex ) junit 테스트 도구들 나는 @Test만 써줬는데 처리는 알아서해줌)
    + 라이브 러리 : 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 라이브러리다.

- 의존관계 주입 (DI)
    + 의존관계는 **정적인 클래스 의존 관계**와 **동적인 객체(인스턴스) 의존관계**로 나누어 생각해야한다.
    + 정적인 클래스 의존관계는 실행을 안해도 자료형으로 판단할 수 있는 의존관계들이다.
    + 동적인 클래스는 예제에서 Discount처럼 FixDiscount가 올 지 RateDiscount가 올 지 실행해봐야 아는 의존관계를 말한다.
    + 의존관계 주입을 하면 동적인 객체 의존관계를 쉽게 바꿀 수 있다는 장점이 있다.

- IOC 컨테이너, DI 컨테이너
    + Appconfig처럼 객체를 생성하고 의존관계를 연결해 주는 것을 IOC 컨테이너, DI 컨테이너 라고 부른다.
    + 최근에는 DI 컨테이너라고 많이 한다.
    + 또는 어샘블러, 오브젝트 팩토리로도 부른다.

-----  

## **8. Spring으로 변환** ##

Appconfig을 Spring으로 변환하려면 간단하게 할 수 있습니다.

```java
@Configuration
public class Appconfig {

    //ctrl + alt  + m
    @Bean
    public MemberService memberService(){
        return new MemberSerivceImpl(memberRepository());
    }

    @Bean
    public OrderServiceImpl orderService(){
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
```
이렇게 @Configuration, @Bean을 모든 메소드에 붙여줍니다. 스프링 컨테이너에 스프링 빈으로 등록한다는 뜻입니다.  

이러면 메인메소드에서의 호출 방식도 달라지기에 바꿔줍니다.  
MemberApp.java을 바꿔줍니다.

```java
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberSerivceImpl;
import hello.core.member.MemberService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MemberApp {

    //psvm
    public static void main(String[] args) {
        //MemberService memberService = new MemberSerivceImpl();
        //Appconfig appconfig = new Appconfig();
        //MemberService memberService = appconfig.memberService();
        
        //추가사항
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Appconfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        //

        Member member = new Member(1L,"memberA", Grade.VIP);
        memberService.join(member);

        Member findMember = memberService.findMember(1L);
        //sout
        System.out.println("new member = " + member.getName());
        System.out.println("find member = " + findMember.getName());

    }
}
```

제대로 돌아가는 지 실행해봅니다.

![](/assets/img/sample/Spring/kyh_Point/C2/result2.JPG)  

로그가 많이찍히지만 잘 돌아가는 것을 볼 수 있습니다.  

같은 방식으로 orderApp도 수정해 줍니다.

```java
package hello.core;

import hello.core.member.*;
import hello.core.order.Order;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class orderApp {
    public static void main(String[] args) {
        //MemberService memberService = new MemberSerivceImpl();
        //OrderService orderService = new OrderServiceImpl();

        /*
        Appconfig appconfig = new Appconfig();
        MemberService memberService = appconfig.memberService();
        OrderServiceImpl orderService = appconfig.orderService();
        */
        
        //추가 부분
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Appconfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
        //

        Long memberId = 1L;
        Member member = new Member(memberId,"memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 20000);

        System.out.println("order = "+ order);

    }
}

```

![](/assets/img/sample/Spring/kyh_Point/C2/result3.JPG)  

이것또한 잘 실행됩니다.

저는 문득 생각했습니다.  

**코드도 더 길고 기존 방식에서 굳이 이름이 같은 메소드를 찾아서 써줘야하는 이 방식이 뭐가 좋은거지..?**  
> 무료 버전에서는 메소드이름을 찾아서 써줘야하는 것 같습니다. 유료버전은 컨테이너 빈에 등록된 이름을 자동으로 찾아준다고 합니다.  


라고 생각했던 것도 잠시.. 김영한 선생님이 이런 생각을 해야 된다고 하시면서 굉장히 많은 장점이 있고 이제 알려준다고 하시니 다음 포스팅에서 배우면서 쓰겠습니다.  

------  



