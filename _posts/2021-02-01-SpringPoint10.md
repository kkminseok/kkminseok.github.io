---
title: Spring - 7 의존관계 자동 주입 (2)
author: 강민석
date: 2021-02-02 10:00:00 +0800
categories: [Java,3. Spring_김영한_스프링-핵심-원리-기본편]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **1. 조회 빈이 2개 이상 - 문제** ##

만약 DicsountPolicy의 하위 타입인 FixDiscountPolicy, RateDiscountPolicy 둘다 스프링 빈으로 선언하고 의존관계 자동 주입을 실행하면 어떻게 될까?

```java
@Component
public class FixDiscountPolicy implements DiscountPolicy {}
~
@Component
public class RateDiscountPolicy implements DiscountPolicy {}

@Autowired
private DiscountPolicy discountPolicy
```

그리고 테스트를 돌려보면 하나의 빈을 기대했는데, 두개의 빈이 발견되었다고 오류가 나타납니다.

```console
NoUniqueBeanDefinitionException: No qualifying bean of type
'hello.core.discount.DiscountPolicy' available: expected single matching bean
but found 2: fixDiscountPolicy,rateDiscountPolicy
```

스프링 빈을 수동 등록해서 문제를 해결해도 되지만, 의존 관계 자동 주입에서 해결하는 여러 방법이 있습니다.

-----

## **2. @Autowired 필드명, @Qulifier, @Primary** ##

**@Autowired 필드명 매칭**
- @Autowired는 타입 매칭을 시도하고, 이때 여러 빈이 있으면 필드 이름, 파라미터 이름으로 빈 이름을 추가 매칭합니다.

```java
@Autowired
private DiscountPolicy discountPolicy
->
@Autowired
private DiscountPolicy rateDiscountPolicy
```
필드 명이 rateDiscountPolicy이므로 정상 주입됩니다.  

**@Autowired 매칭 정리**
1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드명, 파라미터 명으로 빈 이름 매칭


**@Qualifier 사용**

- 추가 구분자를 붙여주는 방법입니다. 주입시 추가적인 방법을 제공하는 것이고, 빈 이름을 변경하는 것은 아닙니다.

```java
@Component
@Qualifier("mainDiscountPolicy")  //mainDiscountPolicy라는 구분자를 붙여줍니다.
public class RateDiscountPolicy implements DiscountPolicy {}
//
@Component
@Qualifier("fixDiscountPolicy") //다른 구현체에는 fixDiscountPolicy라는 구분자를 붙여줬습니다.
public class FixDiscountPolicy implements DiscountPolicy {}
```

이제 호출하는 부분에서 구분자를 명시하여 호출해주면 됩니다.

```java
//생성자 자동 주입 시
@Autowired
public OrderServiceImpl(MemberRepository memberRepository,@Qualifie("mainDiscountPolicy")DiscountPolicy
discountPolicy) {
 this.memberRepository = memberRepository;
 this.discountPolicy = discountPolicy;
}
//수정자 자동 주입 시
@Autowired
public DiscountPolicy setDiscountPolicy(@Qualifier("mainDiscountPolicy")
DiscountPolicy discountPolicy) {
 return discountPolicy;
}
```

만약 @Qualifier("mainDiscountPolicy")를 못찾으면 해당 이름을 가진 스프링 빈을 추가로 찾습니다. 만약 없으면 NoSuchBeanDefinitionException 예외가 발생합니다.  

**@Primary 사용**
- @Primary는 우선순위를 정하는 방법입니다. 한계가 있지만 보편적으로 쓰는 방법이라고 합니다.

rateDiscountPoliy가 우선순위를 갖도록 애노테이션을 붙여주겠습니다
```java
@Component
@Primary //
public class RateDiscountPolicy implements DiscountPolicy {}
@Component
public class FixDiscountPolicy implements DiscountPolicy {}
```
이렇게 작성하면 호출하는 코드에서는 기존 코드처럼 구현체를 직접 명시안해도 된다는 장점이 있습니다.
또한 @Qualifier는 주입 하는 모든 코드에 해당 애노테이션을 붙여줘야한다는 단점이 있기에 @Primary를 자주 사용합니다.

```java
//생성자
@Autowired
public OrderServiceImpl(MemberRepository memberRepository,
 DiscountPolicy discountPolicy) {
 this.memberRepository = memberRepository;
 this.discountPolicy = discountPolicy;
}
//수정자
@Autowired
public DiscountPolicy setDiscountPolicy(DiscountPolicy discountPolicy) {
 return discountPolicy;
}
```

@Primary와 @Qulifier가 같이 쓰였을 땐 @Qualifier가 우선순위를 갖습니다. 
스프링은 자동보다는 수동이, 넓은 범위의 선택권보다는 좁은 범위의 선택권이 우선순위를 갖기 때문입니다.  

-----

## **3. 애노테이션 직접 만들기** ##

- 애노테이션을 직접 만들어주는 이유는 위의 코드에서 구분자를 명시해주어 컴파일에서 타입 체크가 되지 않습니다.

```java
package hello.core.annotataion;
import org.springframework.beans.factory.annotation.Qualifier;
import java.lang.annotation.*;
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER,
ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Qualifier("mainDiscountPolicy") // <<<--
public @interface MainDiscountPolicy {
}

~~~

@Component
@MainDiscountPolicy
public class RateDiscountPolicy implements DiscountPolicy {}
```

작성했던 OrderServiceImpl도 바꿔줍니다.
```java

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, @MainDiscountPolicy DiscountPolicy discountPolicy){
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

애노테이션에는 상속이라는 개념이 없습니다. 이렇게 여러 애노테이션을 모아서 사용하는 기능은 스프링이 지원해주는 기능입니다.
@Qulifier 뿐만 아니라 다른 애노테이션들도 함께 조합해서 사용할 수 있습니다. 심지어 @Autowired도 재정의할 수 있지만 유지보수에 혼란을 가져다줄 수 있습니다.

커스텀 애노테이션은 협업에서 의사소통을 통해 해결해야하기 때문에 꼭 필요한 곳에만 써야합니다.

------

## **4. 조회와 빈이 모두 필요할 때, List, Map ** ##

의도적으로 스프링 빈이 다 필요한 경우가 있습니다.
예를 들어서 위에서 작성한 할인 서비스를 제공하는데, 클라이언트가 할인의 종류를 선택할 수 있다고 가정하면 다음과 같이 구현할 수 있습니다.

**test코드입니다.**

```java
package hello.core.autowired;

import hello.core.AutoAppConfig;
import hello.core.discount.DiscountPolicy;
import hello.core.member.Grade;
import hello.core.member.Member;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import java.util.List;
import java.util.Map;

import static org.assertj.core.api.Assertions.*;

public class AllBeanTest {

    @Test
    void findAllBean(){
        //컨테이너에 설정들을 넣어줌.
        ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class,DiscountService.class);

        //빈을 가져옴.
        DiscountService discountService = ac.getBean(DiscountService.class);
        //실험할 멤버를 생성
        Member member = new Member(1L, "userA", Grade.VIP);
        // discount 함수에 "fixDiscountPolicy"라는 클래스 명을 아예 넣고 값을 기대함
        int discountPrice = discountService.discount(member,10000,"fixDiscountPolicy");

        // 가져온 값이 천원이어야하는데 맞는가?
        assertThat(discountService).isInstanceOf(DiscountService.class);
        assertThat(discountPrice).isEqualTo(1000);

        //마찬가지로 20000원을 넣고 rateDiscountPolicy라는 클래스 값을 넣음. 값은 20000/10인 2000원이 리턴되어야함.
        int rateDiscountPolicy = discountService.discount(member, 20000, "rateDiscountPolicy");
        assertThat(rateDiscountPolicy).isEqualTo(2000);
    }

    static class DiscountService{
        // 맵으로 스프링빈값들을 가져옴.
        private final Map<String, DiscountPolicy> policyMap;
        private final List<DiscountPolicy> policies;

        @Autowired
        public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
            this.policyMap = policyMap;
            this.policies = policies;
            System.out.println("policyMap = " + policyMap);
            System.out.println("policies = " + policies);
        }

        public int discount(Member member, int price, String discountCode) {
            //입력된 String 값을 기준으로 스프링 빈에서 찾아서 가져옵니다.
            DiscountPolicy discountPolicy = policyMap.get(discountCode);
            return discountPolicy.discount(member,price);
        }
    }
}

```

결과  
![](/assets/img/sample/Spring/kyh_Point/C6/map.JPG)  


-----

슬슬 내용이 많아지니 저도 헷갈리는 부분이 많습니다. 다른사람들의 질문을 보면서 보충하고 있지만, 머릿속에 정리가 안되고 있습니다. 빠르게 한 번 훑고 다시 복습해야할 것 같습니다.

 