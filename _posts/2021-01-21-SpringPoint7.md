---
title: Spring - 5 싱글톤 컨테이너
author: 강민석
date: 2021-01-21 10:00:00 +0800
categories: [Java,3. Spring_김영한_스프링-핵심-원리-기본편]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **1. 웹 애플리케이션과 싱글톤** ##

- 만약에 여러 사용자가 동시에 서버에 요청한다면 스프링 없는 순수한 DI컨테이너는 매 번 객체를 생성할 것 입니다.

테스트를 위해 'SingletonTest'.java 테스트 파일을 만든 후 두 객체를 생성해서 객체참조값이 같은 지 확인하겠습니다.  

```java
package hello.core.singletone;

import hello.core.Appconfig;
import hello.core.member.MemberService;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

public class SingletonTest {

    @Test
    @DisplayName("스프링없는 순수한 DI 컨테이너")
    void pureContainer(){
        Appconfig appconfig = new Appconfig();

        //1. 조회 : 객체 하나 생성
        MemberService memberService1 = appconfig.memberService();

        //2. 조회 : 객체 하나 생성
        MemberService memberService2 = appconfig.memberService();

        //같은지 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        //눈으로 보는건 안좋음 테스트 로직작성
        Assertions.assertThat(memberService1).isNotSameAs(memberService2);

    }
}

```

![](/assets/img/sample/Spring/kyh_Point/C4/single1.JPG)  

보시면은 두 객체의 참조가 달라서 서비스 요청시 매번 생기는 것을 알 수 있습니다.   
이러한 방식은 메모리 낭비가 너무 심합니다.  
따라서 객체가 하나만 생성되고 그 객체를 공유하는 방식으로 설계하면 됩니다.  

-----  

## **2. 싱글톤 패턴** ##

- 클래스의 인스턴스가 딱 1개만 생성될 수 있도록 보장하는 디자인 패턴 입니다.
    + private 생성자를 사용해 새로운 객체가 생기는 것을 막습니다.

테스트를 위해 'SingletonService'.java 파일을 테스트 폴더안에 생성합니다.  

```java
package hello.core.singletone;

public class SingletonService {
    //1. static 영역에 객체를 딱 1개만 생성합니다.
    private static final SingletonService instance = new SingletonService();

    //2. public으로 열어서 객체 인스턴스가 필요하면 getInstance()를 통해 조회하도록 허용합니다.
    public static SingletonService getInstance(){
        return instance;
    }

    //3. private 생성자를 통해 새로운 객체가 생기는 것을 막습니다.
    private SingletonService(){}

    public void logic(){
        System.out.println("싱글톤 로직 객체 출력");
    }
}
```

이제 외부에서 사용하려면 컴파일에러가 나타날 것입니다.  

진짜로 동일한 객체를 공유하는지 테스트해보기 위해 아까 만들어둔 테스트 파일에 singletonServiceTest메소드를 넣습니다.

```java
@Test
    @DisplayName("싱글톤 패턴 테스트")
    void singletonServiceTest(){
        //new SingletonService() private으로 막아놔서 에러가 뜬다.

        //1. 조회 : 객체 호출
        SingletonService singletonService1 =SingletonService.getInstance();

        //2. 조회 : 객체 호출
        SingletonService singletonService2 =SingletonService.getInstance();

        //눈으로 확인
        System.out.println("singletonService1 = " + singletonService1);
        System.out.println("singletonService2 = " + singletonService2);

        //테스트 확인
        Assertions.assertThat(singletonService1).isSameAs(singletonService2);
    }
```

![](/assets/img/sample/Spring/kyh_Point/C4/single2.JPG)  

하지만 이러한 싱글톤 패턴에도 단점이 있습니다.  

- 싱글톤 패턴을 구현하는 코드 자체가 많이 들어간다.
- 의존관계상 클라이언트가 구체 클래스에 의존한다. DIP를 위반한다.
- 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반할 가능성이 높다.
- 테스트하기 어렵다.
- 내부 속성을 변경하거나 초기화 하기 어렵다.
- private 생성자로 자식 클래스를 만들기 어렵다.
- 결론적으로 유연성이 떨어진다.
- 안티패턴으로 불리기도 한다.

스프링은 위의 단점들을 해결한 기술을 제공하고 있습니다.  

-----  


## **3. 싱글톤 컨테이너** ##

스프링 컨테이너는 싱글톤 패턴의 문제점을 해결하면서, 객체 인스턴스를 싱글톤(1개만 생성)으로 관리합니다.  

저희가 생성했던 빈들이 전부 싱글톤 패턴이 적용되어 저장되는 것들 입니다.  

스프링 컨테이너는 싱글톤 컨테이너 역할을 합니다. 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리라 합니다.  

- 더이상 코드를 지저분하게 작성안해도 됩니다.
- DIP, OCP, 테스트, private 생성자로 부터 자유롭게 싱글톤을 사용할 수 있습니다.

테스트를 위해 위에 작성한 테스트 파일에 새로운 테스트를 넣어줍니다.  

전에 빈에 넣어줬던 객체들을 꺼내봅니다.  


```java
    @Test
    @DisplayName("스프링 싱글톤 테스트")
    void singletonServiceSpring(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);
        MemberService memberService1 = ac.getBean("memberService",MemberService.class);
        MemberService memberService2 = ac.getBean("memberService",MemberService.class);

        //같은지 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        //눈으로 보는건 안좋음 테스트 로직작성
        Assertions.assertThat(memberService1).isSameAs(memberService2);
    }
```

![](/assets/img/sample/Spring/kyh_Point/C4/single3.JPG)  

테스트 결과를 보면 객체의 참조값이 동일한 것을 확인할 수 있습니다.  

> Note: 스프링의 기본 빈 등록 방식은 싱글톤이지만, 싱글톤 방식만 지원하는 것은 아니다. 요청할 때 마다 새
로운 객체를 생성해서 반환하는 기능도 제공합니다. 자세한 내용은 뒤에 빈 스코프에서 나옵니다.  


하지만 이러한 싱글톤 방식에도 주의할 점이 있습니다.  

-----  

## **4. 싱글톤 방식의 주의점** ##

싱글톤 방식은 여러 클라이언트가 하나의 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(stateful)하게 설계하면 안됩니다.  

- 무상태(stateless)로 설계해야 합니다.
    + 특정 클라이언트에 의존적인 필드가 있으면 안된다.
    + 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다!
    + 가급적 읽기만 가능해야 한다.
    + 필드 대신에 자바에서 공유되지 않는, 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.


문제점을 보기위해 main에 'StatefulService' 자바 파일을 생성합니다.

```java
package hello.core;

public class StatefulService {
    private int price;

    public void order(String name, int price){
        System.out.println("name = " + name + "price = " + price);
        this.price=price;// 문제 발생지점
    }
    public int getPrice(){
        return price;
    }

}
```

ctrl + shift + t를 눌러 테스트파일을 생성합니다.  

```java
package hello.core;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;

import static org.junit.jupiter.api.Assertions.*;

class StatefulServiceTest {



    @Test
    void StatefulServiceSingleton(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(Testconfig.class);

        //객체 두개 생성
        StatefulService statefulService1 = ac.getBean("statefulService", StatefulService.class);
        StatefulService statefulService2 = ac.getBean("statefulService", StatefulService.class);

        //userA가 만원 주문
        statefulService1.order("userA",10000);

        //userB가 2만원 주문
        statefulService2.order("userB",20000);

        //조회 만원이찍혀야함.
        int price = statefulService1.getPrice();

        //2만원 찍힘.
        System.out.println("price = " + price);

        //테스트 마무리를 위한 코드.
        Assertions.assertThat(price).isEqualTo(20000);
    }



    static class Testconfig{
        @Bean
        public StatefulService statefulService(){
            return new StatefulService();
        }
    }

}
```

실행을 하면 다음과 같이 만원이 찍혀야하는데 2만원이 찍히는 것을 확인할 수 있습니다.

![](/assets/img/sample/Spring/kyh_Point/C4/single4.JPG)  

- price라는 공유 필드를 사용하고, 그 값을 변경해서 나타나는 오류입니다.
- 때문에 항상 스프링 빈은 무상태(stateless)로 설계해야합니다.

위의 문제점을 고치기 위해 main 코드를 바꿔줍니다.  

```java
package hello.core;

public class StatefulService {

    public int order(String name, int price){
        System.out.println("name = " + name + "price = " + price);
        return price;
    }

}

```

테스트 코드도 바꿔줍니다.  

```java
    @Test
    void StatefulServiceSingleton(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(Testconfig.class);

        //객체 두개 생성
        StatefulService statefulService1 = ac.getBean("statefulService", StatefulService.class);
        StatefulService statefulService2 = ac.getBean("statefulService", StatefulService.class);

        //userA가 만원 주문
        int price1 = statefulService1.order("userA", 10000);

        //userB가 2만원 주문
        int price2 = statefulService2.order("userB", 20000);


        //조회
        System.out.println("price = " + price1);

        //테스트 마무리를 위한 코드.
        Assertions.assertThat(price1).isEqualTo(10000);
    }
```

![](/assets/img/sample/Spring/kyh_Point/C4/single5.JPG)  

-----  

## **5. @Configuration과 싱글톤** ##

옛날에 작성한 Appconfig 코드를 보면 이상한 점이 있습니다.

```java
@Configuration
public class Appconfig {

    //ctrl + alt  + m
    @Bean
    public MemberService memberService(){
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderServiceImpl orderService(){
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

....
```

'memberService()'가 호출되면서 memberRepository()가 호출됩니다.그리고 새로운 MemoryMemberRepository를 반환하는데  
'orderService()'에서도 memberRepository()가 호출되면서 MemoryMemberRepository를 반환하는데 이 둘을 달라야 정상입니다.  

확인하기위해 테스트 코드를 작성하겠습니다.  
저 둘의 구현체 코드에 할당된 MemberRepository를 반환하는 메소드를 만들겠습니다.  

```java
    public MemberRepository getMemberRepository(){
        return memberRepository;
    }
```

'ConfigurationSingletonTest' test파일을 만듭니다.  

```java
package hello.core.singletone;

import hello.core.Appconfig;
import hello.core.member.MemberRepository;
import hello.core.member.MemberServiceImpl;
import hello.core.order.OrderServiceImpl;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ConfigurationSingletonTest {

    @Test
    void ConfigurationTest(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        System.out.println("memberRepository = " + memberRepository);
        System.out.println("orderService = " + orderService.getMemberRepository());
        System.out.println("memberService = " + memberService.getMemberRepository());

        //테스트 마무리를 위한
        Assertions.assertThat(orderService.getMemberRepository()).isSameAs(memberRepository);
        Assertions.assertThat(memberService.getMemberRepository()).isSameAs(memberRepository);


    }
}
```

![](/assets/img/sample/Spring/kyh_Point/C4/single6.JPG)  

결과를 보시면 모두 똑같은 memberRepository입니다.  
혹시 두 번 호출이 안되는 것인지 확인하기 위해 Appconfig.java에 새로운 코드를 메소드마다 넣어줍니다.  

```java
    @Bean
    public MemberService memberService(){
        System.out.println("call Appconfig.memberService");
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderServiceImpl orderService(){
        System.out.println("call Appconfig.orderService");
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    @Bean
    public MemberRepository memberRepository() {
        System.out.println("call Appconfig.memberRepository");
        return new MemoryMemberRepository();
    }
```

테스트코드를 실행해봅니다.  

![](/assets/img/sample/Spring/kyh_Point/C4/single7.JPG)    

보시면 각 메소드가 실행되는 것을 확인할 수 있습니다.  

어떤 일인지 알아보겠습니다.

-----  

## **6. @Configuration과 바이트코드 조작** ##

위처럼 이미 생성된 자식에 다시 들어가지 않는 이유는 @Configuration 때문입니다.  

테스트를 위해 새로운 테스트 코드를 작성해보겠습니다.

```java
    @Test
    void configurationDeep(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);

        //Appconfig도 스프링 빈으로 등록됩니다.
        Appconfig bean = ac.getBean(Appconfig.class);

        System.out.println("bean = " + bean.getClass());
    }
```



![](/assets/img/sample/Spring/kyh_Point/C4/single7.JPG)  

맨 밑줄 결과를 보면 뒤에 이상한 것들이 많이 붙은 것을 볼 수 있습니다.  

이것은 내가 만든 클래스가 아니라 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스
를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것입니다.  

**예상하기로는 이미 스프링 컨테이너에 있으면 스프링 컨테이너에서 찾아서 반환해주고 없으면 스프링 컨테이너에 등록하여 반환하는 로직이 있을 것이라고 생각합니다. 이 덕분에 싱글톤이 보장됩니다.**

@Configuration을 없애면 CGLIB 기술이 적용안된 Appconfig이 그대로 스프링 빈에 등록되어 처음에 예상했던대로 매 번 객체를 생성하게 됩니다.  


따라서 스프링빈에 등록되지만, 싱글톤은 보장이 안되기에 스프링 설정 정보는@Configuration을 붙여줘야 합니다.  

-----  

