---
title: Spring - 2 스프링 핵심 원리 1(2)
author: 강민석
date: 2021-01-18 15:00:00 +0800
categories: [Java,3. Spring_김영한_스프링-핵심-원리-기본편]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **5. 회원 도메인 실행과 테스트** ##


- 먼저 눈으로 확인해보기 위해 일반적으로 안좋은 테스트하는 방법을 소개하겠습니다.

![](/assets/img/sample/Spring/kyh_Point/C1/app.JPG)  

이렇게 java파일을 만들어주고


```java
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberSerivceImpl;
import hello.core.member.MemberService;

public class MemberApp {

    //psvm
    public static void main(String[] args) {
        MemberService memberService = new MemberSerivceImpl();
        Member member = new Member(1L,"memberA", Grade.VIP);
        memberService.join(member);

        Member findMember = memberService.findMember(1L);
        //sout
        System.out.println("new member = " + member.getName());
        System.out.println("find member = " + findMember.getName());

    }
}
```

임의로 멤버 객체를 넣어주고 find()함수를 통해 같은지 확인합니다.  

별로 좋은 테스트가 아닙니다. 이번엔 Junit을 이용한 테스트 방법입니다.  

![](/assets/img/sample/Spring/kyh_Point/C1/testfold.JPG)  

해당 위치에 패키지와 test.java파일을 만들어줍니다.  

```java
package hello.core.member;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class MemberServiceTest {

    MemberService memberService = new MemberSerivceImpl();
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
이렇게 작성하고 테스트를 돌리면 초록불이 뜨면서 테스트가 완료됩니다.  
이렇게 작성한 자바코드들은 문제를 띄고있습니다.  

저번 포스트에 작성한 자바 코드를 보면
```java
public class MemberSerivceImpl implements MemberService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    ~~
}
```

이런 코드가 있는데 MemberServiceImpl은 MemberRepository 추상화에 의존하고 있지만 MemberRepository는 new로 생성한 MemoryMemberRepository() 객체에 의존하는 문제점이 있습니다. 

이러한 문제점은 '주문'까지 만들고 고친다고 합니다.

-----  

## **6. 주문과 할인 도메인 설계** ##

**회원 도메인 설계와 마찬가지로 주문과 할인에 대한 클래스 다이어그램 등 강의자료를 그대로 쓸 순 없으니 넘어가겠습니다.추후 제가 따로 작성해서 올리겠습니다.**

-----  

## **7. 주문과 할인 도메인 개발** ##

먼저 할인 정책에 대한 인터페이스를 작성하겠습니다.

![](/assets/img/sample/Spring/kyh_Point/C1/discount.JPG)  

위의 경로에 인터페이스 파일을 만들어줍니다.

```java
package hello.core.discount;

import hello.core.member.Member;

public interface DiscountPolicy {
    /*
    return : 할인 금액
     */
    int discount(Member member, int price);
}

```

똑같은 경로에 FixDiscountPolicy.java 파일을 만들어주고 구현합니다.

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class FixDiscountPolicy implements DiscountPolicy{

    private int discountFixAmount = 1000;
    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP)
            return discountFixAmount;
        else
            return 0;

    }
}
```

회원등급이 VIP인 경우에 고정된 할인금액을 리턴하는 함수입니다.  

그 다음 회원이 주문을 하므로 주문 서비스를 만들어야합니다.  

order 패키지를 만들어주고 도메인을 담기위한 Order.java 파일을 만들어줍니다.  

- 밑의 코드는 memberId, itemName, itemPrice, discountPrice의 get(), set(), 생성자, toString()-> 출력용 함수를 만들어준 것 입니다.
- **Alt + Insert 애용합시다.**

```java
package hello.core.order;

public class Order {

    private Long memberId;
    private String itemName;
    private int itemPrice;
    private int discountPrice;

    public int calculatePrice(){
        return itemPrice-discountPrice;
    }

    @Override
    public String toString() {
        return "Order{" +
                "memberId=" + memberId +
                ", itemName='" + itemName + '\'' +
                ", itemPrice=" + itemPrice +
                ", discountPrice=" + discountPrice +
                '}';
    }

    public Long getMemberId() {
        return memberId;
    }

    public void setMemberId(Long memberId) {
        this.memberId = memberId;
    }

    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    public int getItemPrice() {
        return itemPrice;
    }

    public void setItemPrice(int itemPrice) {
        this.itemPrice = itemPrice;
    }

    public int getDiscountPrice() {
        return discountPrice;
    }

    public void setDiscountPrice(int discountPrice) {
        this.discountPrice = discountPrice;
    }

    public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
        this.memberId = memberId;
        this.itemName = itemName;
        this.itemPrice = itemPrice;
        this.discountPrice = discountPrice;
    }

}

```


그리고 서비스를 위한 OrderService 인터페이스 파일을 만들어줍니다.

```java
package hello.core.order;

public interface OrderService {
    Order createOrder(Long memberId,String itemName, int itemPrice);
}

```

OrderServiceImpl.java 파일을 만들어줍니다.

```java
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;
import hello.core.member.MemoryMemberRepository;

public class OrderServiceImpl implements OrderService{
    MemberRepository memberRepository = new MemoryMemberRepository();
    DiscountPolicy discountPolicy = new FixDiscountPolicy();

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice= discountPolicy.discount(member,itemPrice);
        return new Order(memberId,itemName,itemPrice,discountPrice);
    }
}

```

위의 코드는 설계가 잘 된 코드라고 합니다. 어떤 한개를 고친다할 때 그 객체 외 다른 객체는 건들지 않기 때문이라고 합니다.  

최종 폴더 상황

![](/assets/img/sample/Spring/kyh_Point/C1/result2.JPG)  

-----  

## **8. 주문과 할인 도메인 테스트** ##

- 저번에 작성했던 메인메소드로 테스트하는 코드입니다.

orderApp.java 파일을 만들어줍니다.

```java
package hello.core;

import hello.core.member.*;
import hello.core.order.Order;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;

public class orderApp {
    public static void main(String[] args) {
        MemberService memberService = new MemberSerivceImpl();
        OrderService orderService = new OrderServiceImpl();

        Long memberId = 1L;
        Member member = new Member(memberId,"memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);

        System.out.println("order = "+ order);

    }
}

```
- 새로운 멤버를 만들어서 아이템 주문을 한다음 해당 객체에 들어있는 값들을 출력합니다.

![](/assets/img/sample/Spring/kyh_Point/C1/result3.JPG)  
위의 사진처럼 discountPrice 변수에 1000이 들어있으면 됩니다.

다음은 Junit을 사용한 테스트 입니다.

![](/assets/img/sample/Spring/kyh_Point/C1/ordertst.JPG)  

해당 경로에 package와 test파일을 만들어주고 

```java
package hello.core.order;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberSerivceImpl;
import hello.core.member.MemberService;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class OrderServiceTest {
    MemberService memberService = new MemberSerivceImpl();
    OrderService orderService = new OrderServiceImpl();

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

새로운 멤버를 넣고 Assetions을 이용하여 테스트를 진행하면 녹색불이 뜹니다.  
![](/assets/img/sample/Spring/kyh_Point/C1/testsuc.JPG)  

다음 강의에서는 만약 할인정책이 다른 객체를 넣으면 문제가 발생하는 지 발생하면 어떤 문제가 발생하는 지 보겠습니다.  

-----  


