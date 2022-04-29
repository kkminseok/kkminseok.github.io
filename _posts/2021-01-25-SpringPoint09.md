---
title: Spring - 7 의존관계 자동 주입 (1)
author: 강민석
date: 2021-01-25 20:00:00 +0800
categories: [Java,3. Spring_김영한_스프링-핵심-원리-기본편]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **1. 다양한 의존관계 주입 방법** ##

의존관계주입은 크게 4가지 방법이 있습니다.  

- 생성자 주입
- 수정자 주입
- 필드 주입
- 일반 메서드 주입

**생성자 주입**

- 지금까지 한 방법으로 생성자를 통해서 의존관계를 주입받는 방법입니다.
- 생성자 호출시점에 딱 1번만 호출되는 것이 보장되어 '불변,필수' 의존관계에 사용됩니다.

```java
@Component
public class OrderServiceImpl implements OrderService{
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

### **생성자가 딱 1개만 있으면 @Autowired를 생략해도 자동 주입됩니다.** ###

```java
@Component
public class OrderServiceImpl implements OrderService{
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

**수정자 주입**

- setter라 불리는 필드의 값을 변경하는 메서드를 통해 의존관계를 주입합니다.
- 선택, 변경 가능성이 있는 의존관계에 사용하고 자바빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 방법입니다.  

- 자바빈 프로퍼티 규약이란 get(),set() 메서드를 통해 값을 읽거나 수정하는 규칙을 말합니다. 검색하면 더 자세한 내용을 알 수 있습니다.  


```java
@Component
public class OrderServiceImpl implements OrderService{
    private  MemberRepository memberRepository;
    private  DiscountPolicy discountPolicy;

    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }

    @Autowired
    public void setMemberRepository(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }
    
}
```

> @Autowired의 기본 동작은 주입할 대상이 없으면 오류가 발생합니다.  
    주입할 대상이 없어도 동작하게 하려면 @Autowired(required = false)로 지정합니다.

**필드 주입**

- 필드에 주입하는 방법입니다.
- 추천하지 않는 방식으로 DI 프레임워크가 없으면 아무것도 할 수 없고, 외부에서 변경이 불가능해서 테스트하기 어렵다는 단점이 있습니다.
    + 순수 자바로 테스트를 돌릴 수가 없다는 단점이 있습니다.
- 애플리케이션의 실제 코드와 관계 없는 테스트 코드나 특별한 상황에서 사용됩니다.


```java
@Component
public class OrderServiceImpl implements OrderService{
    @Autowired
    private  MemberRepository memberRepository;
    @Autowired
    private  DiscountPolicy discountPolicy;
}
```

**일반 메서드 주입**

- 일반 메서드를 통해 주입 받는 방법입니다.
- 한번에 여러 필드를 주입 받을 수 있지만, 일반적으로 잘 사용하지 않습니다.
    + 방식이 set주입, 생성자 주입과 비슷하여 잘 안쓰인다고 합니다.

```java
@Component
public class OrderServiceImpl implements OrderService {
    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;

    @Autowired
    public void init(MemberRepository memberRepository, DiscountPolicy
discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

-----  

## **2. 옵션처리** ##

주입할 스프링 빈이 없어도 동작해야할 때가 있습니다.  

하지만 @Autowired만 사용하면 'required'옵션의 기본값이 true로 되어 있어서 자동 주입 대상이 없으면 오류를 발생시킵니다.  

자동 주입 대상을 옵션으로 처리하는 방법은 다음 세가지가 있습니다.

- @Autowired(required = false) : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출이 안됩니다.
- org.springframework.@Nullable : 자동 주입할 대상이 없으면 null이 입력됩니다.
- Optional<> : 자동 주입할 대상이 없으면 Optional.empty가 입력됩니다.

테스트를 위해 'AutowiredTest'.java 파일을 만들고 다음 코드를 테스트 해봅니다.

```java
package hello.core.autowired;

import hello.core.member.Member;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.lang.Nullable;

import java.util.Optional;

public class AutowiredTest {

    @Test
    void Test(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(Testconfig.class);
    }

    static class Testconfig{

        //Member class는 스프링이 관리안하고 있습니다.

        //1. required옵션에 false를 넣어줌
        @Autowired(required = false)
        public void setNoBean1(Member member){
            System.out.println("setNobean1 = " + member);
        }

        //2. null호출
        @Autowired
        public void setNoBean2(@Nullable Member member){
            System.out.println("setNobean2 = " + member);
        }

        //3. Optional.empty 호출
        @Autowired
        public void setNoBean3(Optional<Member> member){
            System.out.println("setNobean3 = " + member);
        }


    }
}
```

결과를 보시면 1번의 경우 메서들 아예 호출을 안하는 것을 볼 수 있습니다.  

![](/assets/img/sample/Spring/kyh_Point/C6/option.JPG)  

-----  

## **3. 생성자 주입을 선택해라** ##

생성자 주입을 사용해야하는 이유를 설명합니다.  

**불변**


- 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다고 합니다. 오히려 변하면 안된다고 합니다.  
- 수정자 주입을 사용하면 set()메서드를 public으로 열어둬야하는 단점이 있습니다. 또한 누군가 실수로 변경할 수 도 있고, 변경하면 안되는 메서드를 열어두는 것은 좋은 설계 방법이 아닙니다.
- 생성자 주입은 객체를 생성할 때 딱 1번 호출되는게 보장되므로 불변하게 설계할 수 있습니다.

**누락**


```java
@Test
void createOrder() {
 OrderServiceImpl orderService = new OrderServiceImpl();
 orderService.createOrder(1L, "itemA", 10000);
}
```

- 프레임워크 없이 순수한 자바 코드를 수정자 주입을 적용하고 테스트를 돌릴려고 하면 createOrder()함수를 실행하기 위해 더미 객체라도 넣어줘야합니다. 그러지 않으면 의존 관계가 주입되지 않아 Null Point Exception이 발생합니다.
- 생성자 주입을 사용하면 위의 코드는 **컴파일 오류**가 발생합니다. 그렇기에 어떤 값의 의존관계 주입이 누락되었는 지 확인할 수 있습니다.

**final 키워드**

- 생성자 주입을 사용하면 필드에 'final' 키워드를 사용할 수 있습니다. 그래서 생성자에서 혹시라도 값이 설정되지 않는 오류를 컴파일 시점에 막아줍니다.  
> 컴파일 오류가 가장 빠르고 좋은 오류라고 합니다.

**정리**
- 생성자 주입을 사용하면 프레임워크에 의존하지 않고, 순수한 자바 언어의 특징을 잘 살리는 방법이기도 합니다.
- 기본으로 생성자 주입을 사용하고, 필수 값이 아닌 경우에는 수정자 주입 방식을 옵션으로 부여하면 됩니다. 
- 항상 생성자 주입을 선택하고 필드 주입은 사용하지 않는게 좋습니다.

-----  


## **4. 롬북과 최신 트랜드** ##

- 필드 주입처럼 한줄로 의존관계를 주입하는 방법이 있습니다.
- 롬북 라이브러리를 설정하기 전에 기본 코드를 바꿔줄 수 있습니다.

```java
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    //생성자가 하나일 땐 Autowired 생략가능
    //@Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

그 다음 룸복 라이브러리를 적용하기 위해, build.gradle 설정을 바꿔줍니다.

```java
plugins {
	id 'org.springframework.boot' version '2.4.2'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	//lombok 라이브러리 추가 시작
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testCompileOnly 'org.projectlombok:lombok'
	testAnnotationProcessor 'org.projectlombok:lombok'
	//lombok 라이브러리 추가 끝

	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

test {
	useJUnitPlatform()
}

```
이후 File -> settings에서 plugins을 검색해준 다음 lombok을 설치해줍니다.  

![](/assets/img/sample/Spring/kyh_Point/C6/lombok.JPG)  

Annotation Processors를 검색한 다음 Enable annotation~ 을 체크해줍니다.  

![](/assets/img/sample/Spring/kyh_Point/C6/lombok2.JPG)  

그리고 기존 코드를 다음과 같이 바꿔주면 끝납니다.  

```java
//추가!
@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService{
    //final 붙은 것을 가지고 생성자를 알아서 만들어줌.
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
}
```
> 최근에는 생성자를 1개 두고, @Autowired를 생략하는 방법을 주로 사용합니다. 여기에 lombok라이브러리의 @RequiredArgConstructor를 사용하면 기능은 제공하면서, 코드는 깔끔하게 사용할 수 있습니다.  

또한 lombok은 Getter,Setter, 등을 제공하고 있는데 간단히 테스트 코드를 작성하면

```java
package hello.core;


import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class Lombok {
    
    private int number;
    private String name;
    
    public static void main(String[] args) {
        Lombok lombok = new Lombok();
        lombok.setName("hi");
        String temp = lombok.getName();
        System.out.println("lombok = " + lombok + temp);
    }
}

```

![](/assets/img/sample/Spring/kyh_Point/C6/lombok3.JPG)  

이렇듯 롬북을 이용하면 Get(),Set()함수를 안만들어도 되는 대단한 기능을 제공합니다.

