---
title: Spring - 3 스프링 핵심 원리 - 객체 지향 원리 적용(1)
author: 강민석
date: 2021-01-19 09:00:00 +0800
categories: [Java,3. Spring_김영한_스프링-핵심-원리-기본편]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **1. 새로운 할인 정책 적용** ##

**저번에 작성한 무조건 1천원 할인을 구매 가격의 10%할인으로 바꾼다고 했을 때 객체 지향으로 코드를 작성하지 않았으면 뜯어 고쳤어야했습니다.**  
**interface 파일에 맞게 새로운 클래스만 작성해주면 거기에 끼우면 원하는 할인 정책을 적용할 수 있습니다.**


discount 패키지 안에 'RateDiscountPolicy' java파일을 만듭니다.  

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class RateDiscountPolicy implements DiscountPolicy{

    private int discountpercent = 10;
    // ctrl + shift + T = test작성
    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP){
            return price * discountpercent / 100;
        }
        else
            return 0;
    }
}
```

- vip일 때 price는 들어온 값의 할인금액을 리턴하고 있습니다. 

- 테스트를 작성해보겠습니다. 위의 주석처럼 해당 단축키를 눌러 테스트 파일을 만듭니다.

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class RateDiscountPolicyTest {

    RateDiscountPolicy rateDiscountPolicy = new RateDiscountPolicy();
    @Test
    @DisplayName("VIP면 10% 할인되어야 한다.")
    void vip_o(){
        //given
        Member member = new Member(1L,"memberVIP", Grade.VIP);
        //when
        int discount = rateDiscountPolicy.discount(member,10000);
        //then
        //Assertions에 Alt + Enter로 줄이기
        assertThat(discount).isEqualTo(1000);
    }

    //VIP가 아닐 때
    @Test
    @DisplayName("VIP가 아니면 할인되지 않아야한다.")
    void vip_x(){
        //given
        Member member = new Member(1L,"memberBASIC",Grade.BASIC);
        //when
        int discount = rateDiscountPolicy.discount(member,10000);
        //then
        assertThat(discount).isEqualTo(1000);
    }

}
```

테스트를 돌려보면 밑의 코드에서 0을 기대하고 있지만 1000이라는 값이 들어와 테스트가 돌아가지 않는 것을 볼 수 있습니다.

![](/assets/img/sample/Spring/kyh_Point/C2/result1.JPG)  

**@DisplayName은 테스트 이름이 한글로 보일 수 있게 도와줍니다. 만약 적용이 안되면 setting에서 build를 Grale -> Intellij IDEA로 바꾸셔야합니다.**

![](/assets/img/sample/Spring/kyh_Point/C2/display.JPG)  

-----  

## **2. 새로운 할인 정책과 문제점** ##

- 위의 코드를 돌아가게 하려면, 'OrderSerivceImpl'을 밑처럼 고치면 됩니다.

```java
public class OrderServiceImpl implements OrderService{
    MemberRepository memberRepository = new MemoryMemberRepository();
    DiscountPolicy discountPolicy = new RateDiscountPolicy();
```

**여기서 문제가 발생합니다.**  
**수정해야할 것이 OrderServiceImpl의 코드이고 그 코드가 구현체를 바꾸는 코드이기 때문입니다.**  
**DIP는 추상화에 따라야하는데 구현체에 의존하는 위의 코드는 DIP를 위반하는 행위입니다.**  
**DIP를 준수하기 위해 밑의 코드처럼 바꿔줘야합니다.**  

```java
public class OrderServiceImpl implements OrderService{
    MemberRepository memberRepository = new MemoryMemberRepository();
    //discount만 보세요 위는 무시
    DiscountPolicy discountPolicy;
```

하지만 위의 코드는 돌아가지 않습니다. 아무것도 할당되지 않았기 때문입니다.  
그래서 누군가가 클라이언트인 OrderSeviceImpl에 DiscountPolicy 구현 객체를 대신 생성하고 주입해줘야 합니다.  

-----  

## **3. 관심사 분리** ##

위의 코드의 문제는 OrderServiceImpl 클래스에서 discount 객체와 memberRepository 객체를 정해주는 책임을 갖고 있습니다.  
OrderService에 대한 서비스만 실행하는 클래스가 다른 객체를 임의로 정해서 실행한다는 것은 **다양한 책임**을 지니고 있습니다.  
관심사를 분리하기 위해 **구현 객체를 생성**하고 **연결**하는 책임을 가진 별도의 설정 클래스를 만듭니다.

Appconfig.java 파일을 만듭니다.  

```java
package hello.core;

import hello.core.discount.FixDiscountPolicy;
import hello.core.member.MemberSerivceImpl;
import hello.core.member.MemberService;
import hello.core.member.MemoryMemberRepository;
import hello.core.order.OrderServiceImpl;

public class Appconfig {

    public MemberService memberService(){
        return new MemberSerivceImpl(new MemoryMemberRepository());
    }

    public OrderServiceImpl orderService(){
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
    }
}
```

- 위의 코드를 설명하기 전에 먼저 위에서 작성한 MemberServiceImpl class와 OrderServiceImpl을 저 코드에 맞게 수정해줍니다.  

- MemberServiceImpl

```java
package hello.core.member;

public class MemberSerivceImpl implements MemberService{

    private final MemberRepository memberRepository;

    public MemberSerivceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

- OrderServiceImpl

```java
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;

public class OrderServiceImpl implements OrderService{
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice= discountPolicy.discount(member,itemPrice);
        return new Order(memberId,itemName,itemPrice,discountPrice);
    }
}

```
- OrderServiceImpl 코드를 보면 Order클래스는 discountPolicy에 어떤 구현체가 오던지 신경을 안쓰고 있습니다.
- 생성자를 통해 어떤 객체를 주입받는 지는 Appconfig 클래스에서 결정됩니다.
- 위의 클래스들은 이제 **실행에만 집중하면 되고 의존관계에 대한 고민은 외부에 맡기면 됩니다.**
- 객체의 생성과 연결은 Appconfig이 담당하고 위의 클래스들은 이제 추상화에만 의존하므로 DIP를 위배하지 않습니다.
- **Appconfig은 MemoryMemberRepository 객체를 생성하고 그 참조값을 MemberServiceImpl을 생성하면서 생성자로 전달합니다.**
- **MemberServiceImpl 입장에서 의존관계를 마치 외부에서 주입하는 것과 같아서 DI(Dependency Injection), 의존관계 주입, 의존성 주입이라고 부릅니다.**
- 위의 코드를 테스트 하기위해 작성했던 MemberApp코드를 바꿔줍니다.

```java
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;

public class MemberApp {

    //psvm
    public static void main(String[] args) {
        //MemberService memberService = new MemberSerivceImpl();
        //추가된 사항 이제 매번 Appconfig 객체를 생성해서 필요한것만 찾아 가져오면 됩니다.
        Appconfig appconfig = new Appconfig();
        MemberService memberService = appconfig.memberService();
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

마찬가지로 OrderApp도 수정해줍니다.  

```java
package hello.core;

import hello.core.member.*;
import hello.core.order.Order;
import hello.core.order.OrderServiceImpl;

public class orderApp {
    public static void main(String[] args) {
        //MemberService memberService = new MemberSerivceImpl();
        //OrderService orderService = new OrderServiceImpl();
        //추가된 코드
        Appconfig appconfig = new Appconfig();
        MemberService memberService = appconfig.memberService();
        OrderServiceImpl orderService = appconfig.orderService();
        //

        Long memberId = 1L;
        Member member = new Member(memberId,"memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);

        System.out.println("order = "+ order);

    }
}

```

테스트 코드들도 수정해줍니다. MemberServiceTest를 수정해줍니다.

```java
package hello.core.member;

import hello.core.Appconfig;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class MemberServiceTest {

    //추가 코드
    MemberService memberService;
    //테스트 수행전에 실행됩니다.
    @BeforeEach
    public void beforeEach(){
        Appconfig appconfig = new Appconfig();
        memberService = appconfig.memberService();
        //선언과 대입을 따로하는 이유는 좀 더 직관적으로 무엇을 테스트하는 지 보기 위함입니다.
    }

    //

    @Test
    void join(){
        //given
        Member member = new Member(1L,"memberA",Grade.VIP);
        //when
        memberService.join(member);
        Member findmember = memberService.findMember(1L);

        //then
        Assertions.assertThat(findmember).isEqualTo(member);
    }
}

```

마찬가지로 orderTest도 수정해줍니다.

```java
package hello.core.order;

import hello.core.Appconfig;
import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberSerivceImpl;
import hello.core.member.MemberService;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class OrderServiceTest {
//    MemberService memberService = new MemberSerivceImpl();
 //   OrderService orderService = new OrderServiceImpl();
    MemberService memberService;
    OrderService orderService;

    @BeforeEach
    public void beforeEach(){
        Appconfig appconfig = new Appconfig();
        memberService = appconfig.memberService();
        orderService = appconfig.orderService();
    }


    @Test
    void CreateOrder(){
        Long memberId=1L;
        Member member = new Member(memberId,"memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId,"itemA",10000);
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
    }
}

```


- APPconfig을 통해 관심사를 확실하게 분리하였습니다.
- 각각의 서비스는 이제 실행에만 집중하면 됩니다. 

------  

## **4. Appconfig 리팩터링** ##

기존 Appconfig을 리팩터링 해야합니다.  
- new MemoryMemberRespoitory라는 중복이 있고, 한 눈에 들어오지 않습니다.

```java
package hello.core;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.member.MemberRepository;
import hello.core.member.MemberSerivceImpl;
import hello.core.member.MemberService;
import hello.core.member.MemoryMemberRepository;
import hello.core.order.OrderServiceImpl;

public class Appconfig {

    //ctrl + alt  + m
    public MemberService memberService(){
        return new MemberSerivceImpl(memberRepository());
    }

    public OrderServiceImpl orderService(){
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
    public DiscountPolicy discountPolicy(){
        return new FixDiscountPolicy();
    }
}

```

- 이제 한 눈에 각 함수들이 무슨 역할을 하는지 알 수 있고, 중복도 제거된 Appconfig을 만들었습니다.

-----  

## **5. 새로운 구조와 할인 정책 적용** ##

- Test를 돌려보기위해서는 한개의 코드만 수정하면 됩니다.

- Appconfig에서 다음 코드만 바꿔주면 원하는 할인정책으로 적용되는 것을 볼 수 있습니다.

```java
   public DiscountPolicy discountPolicy(){
        //return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
```

-----  
