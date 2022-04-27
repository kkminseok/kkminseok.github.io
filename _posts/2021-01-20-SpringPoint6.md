---
title: Spring - 4 스프링 컨테이너와 스프링 빈
author: 강민석
date: 2021-01-20 10:00:00 +0800
categories: [Java,3. Spring_김영한_스프링-핵심-원리-기본편]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **1. 스프링 컨테이너 생성** ##

```java
ApplicationContext applicationContext =
 new AnnotationConfigApplicationContext(AppConfig.class);
```

- ApplicationContext를 스프링 컨테이너라고 한다. 또한 인터페이스입니다. (다형성이 적용 됨.)
- 스프링 컨테이너는 XML로 만들 수도 있고 애노테이션 기반 자바 설정파일로 만들 수 있습니다. (최근에는 애노테이션 기반을 많이 씀.)
- Appconfig을 사용했던 방식이 애노테이션 기반 자바 설정파일로 만든 것입니다.
- new AnnotationConfigApplicationContext는 ApplicationContext 인터페이스의 구현체입니다.
- 스프링 컨테이너는 Bean Factory, ApplicationContext로 나뉘지만 Bean Factory를 직접적으로 쓰는 경우가 거의 없어서 일반적으로 ApplicationContext를 스프링 컨테이너라고 합니다. 

- 스프링 컨테이너 생성 과정은 다음과 같습니다.
    + 1.스프링 컨테이너 생성. (Appconfig과 같이 구성 정보 지정 필요)
    + 2.스프링 빈 등록 (빈 이름을 직접 부여할 수 있다. 이름이 중복되서는 안된다.)
    + 3.스프링 빈 의존관계 설정 - 준비
    + 4.스프링 빈 의존관계 설정 - 완료 (설정을 참고하여 의존관계를 주입한다. )

-----  

## **2. 컨테이너에 등록된 모든 빈 조회** ##

패키지명은 아무렇게 하고 'ApplicationContextInfoTest' java파일을 test폴더 안에 만들어줍시다.

```java
package hello.core.bean;

import hello.core.Appconfig;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

class ApplicationContextInfoTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);

    @Test
    @DisplayName("모든 빈 출력하기")
    void findAllbean(){
        //alt + enter하면 반환형에 맞는 자료형, 변수 넣어줍니다.
        //ac 컨테이너 안에 들어있는 빈들의 모든 이름을 String에 넣습니다.
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        //iter 를 입력 후 tab키를 누르면 자동으로 for문이 만들어집니다.
        for (String beanDefinitionName : beanDefinitionNames) {
            //정의된 이름을 하나하나 꺼내면서 bean이라는 Object자료형을 가진 변수에 넣어줍니다.
            Object bean = ac.getBean(beanDefinitionName);
            //출력
            System.out.println("name = " + beanDefinitionName + "object = " + bean);
        }
    }

    @Test
    @DisplayName("애플리케이션 빈 출력하기")
    void findApplicationBean(){
        //마찬가지로 정의된 빈의 이름들을 가져옵니다.
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            //BeanDefinition 자료형을 반환합니다.
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);
            //Role ROLE_APPLICATION: 직접 등록한 애플리케이션 빈
            //Role ROLE_INFRASTRUCTURE: 스프링이 내부에서 사용하는 빈
            if(beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION){
                Object bean = ac.getBean(beanDefinitionName);
                System.out.println("name = " + beanDefinitionName + "object = " + bean);
            }
        }
    }
}

```

- findApplicationBean()안에 ROLE_APPLICATION을 테스트했을 때입니다.
![](/assets/img/sample/Spring/kyh_Point/C3/beanresult.JPG)  

- findApplicationBean()안에 ROLE_INFRASTRUCTURE을 테스트했을 때입니다.

![](/assets/img/sample/Spring/kyh_Point/C3/beanresult2.JPG)  

## **3. 스프링 빈 조회 - 기본** ##

- 스프링 컨테이너에서 스프링 빈을 찾는 가장 기본적인 방법입니다.  
```java
ac.getBean(빈이름, 타입);
ac.getBean(타입);
```
- 조회 대상이 없으면 다음과 같은 예외를 발생시킵니다. 
    + NoSuchBeanDefinitionException: No bean named 'xxxxx' available

예제로 보기위해 Test 폴더 안에 임의의 패키지를 만들고 'ApplicationContextBasicFindTest'.java test파일을 만들어줍니다. 

```java
package hello.core.bean;

import hello.core.Appconfig;
import hello.core.member.MemberSerivceImpl;
import hello.core.member.MemberService;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoSuchBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

class ApplicationContextBasicFindTest {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);

    @Test
    @DisplayName("빈 이름, 타입 조회")
    void findBeanByName(){
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        Assertions.assertThat(memberService).isInstanceOf(MemberSerivceImpl.class);
    }

    @Test
    @DisplayName("타입으로 조회")
    void findBeanByType(){
        MemberSerivceImpl memberService = ac.getBean(MemberSerivceImpl.class);
        Assertions.assertThat(memberService).isInstanceOf(MemberSerivceImpl.class);
    }

    //안좋은 방법이지만 -> 구체에 의존하므로 ,참고용으로
    @Test
    @DisplayName("구체 타입으로 조회")
    void findBeanByName2(){
        MemberSerivceImpl memberService = ac.getBean("memberService", MemberSerivceImpl.class);
        Assertions.assertThat(memberService).isInstanceOf(MemberSerivceImpl.class);
    }

    //이름이 없을 때 테스트
    @Test
    @DisplayName("빈 이름으로 조회 x")
    void findBeanByNameX(){
        //MemberSerivceImpl xxxxx = ac.getBean("XXXXX", MemberSerivceImpl.class);
        //가급적 밑의 Assertions도 Alt + Enter로 생랼하자.
        org.junit.jupiter.api.Assertions.assertThrows(NoSuchBeanDefinitionException.class,
                () ->ac.getBean("XXXXX", MemberSerivceImpl.class));
    }
}
```

-----  

## **4. 스프링 빈 조회 - 동일한 타입이 둘 이상** ##

- 타입으로 조회 시 같은 타입의 스프링 빈이 둘 이상 존재하면 오류를 발생시킬 수 있습니다.
- 테스트를 위해 Test 폴더 안에 'ApplicationContextSameBeanFindTest'.java 테스트 파일을 만듭니다.

```java
package hello.core.bean;

import hello.core.member.Member;
import hello.core.member.MemberRepository;
import hello.core.member.MemoryMemberRepository;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.Map;

class ApplicationContextSameBeanFindTest {
    //밑의 설정을 컨테이너에 등록해줍니다.
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SameBeanConfig.class);

    //테스트 파일 안에서만 사용할 것.
    @Configuration
    static class SameBeanConfig{

        @Bean
        public MemberRepository memberRepository1(){
            return new MemoryMemberRepository();
        }

        @Bean
        public MemberRepository memberRepository2(){
            return new MemoryMemberRepository();
        }
    }

    //같은 타입이 둘 이상 있으면 오류를 발생.
    @Test
    @DisplayName("타입으로 조회시 같은 타입이 둘 이상 있으면, 중복 오류가 발생한다")
    void findBeanByTypeDuplicate(){
        //ac.getBean(MemberRepository.class)를 하면 오류가 발생
        //그냥 조회하면 밑과같은 오류(NoUniqueBeanDefinitionException)가 뜨므로 해당 오류를 넣어서 테스트를 완료한다.
        Assertions.assertThrows(NoUniqueBeanDefinitionException.class,
                () -> ac.getBean(MemberRepository.class));
    }

    @Test
    @DisplayName("특정 타입을 모두 조회하기")
    void findBeanByType(){
        //Alt + Enter 애용!!
        Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
        //iter + tab키
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + "value = " + beansOfType.get(key));
        }
        System.out.println("beansOfType = " + beansOfType);
        org.assertj.core.api.Assertions.assertThat(beansOfType.size()).isEqualTo(2);
    }

}

```

![](/assets/img/sample/Spring/kyh_Point/C3/result3.JPG)  

-----  

## **5. 스프링 빈 조회 - 상속 관계** ##

- 스프링 빈이 상속관계로 이루어져 있을 경우 부모 타입을 조회할 경우 모든 자식들이 조회됩니다.
- 따라서 모든 객체들의 상위 객체인 'Object' 타입으로 조회 시, 모든 스프링 빈을 조회할 수 있습니다.

![](/assets/img/sample/Spring/kyh_Point/C3/bean.JPG)  

> 출처 : 김영한 선생님의 강의자료

- 예제코드를 작성하기 위해 새로운 테스트 파일인 'ApplicationContextExtendsFindTest'.java 테스트 파일을 만듭니다. 

```java
package hello.core.bean;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.discount.RateDiscountPolicy;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.Map;

class ApplicationContextExtendsFindTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);

    @Configuration
    static class TestConfig{

        @Bean
        public DiscountPolicy rateDiscountPolicy(){
            return new RateDiscountPolicy();
        }

        @Bean
        public DiscountPolicy fixDiscountPolicy(){
            return new FixDiscountPolicy();
        }
    }


    @Test
    @DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 중복 오류가 발생한다")
    void findBeanByParentDuplicate(){
        //이 코드는 예외를 발생
        // Map<String, DiscountPolicy> beansOfType = ac.getBeansOfType(DiscountPolicy.class);
        Assertions.assertThrows(NoUniqueBeanDefinitionException.class,
                () -> ac.getBean(DiscountPolicy.class));
    }

}
```

![](/assets/img/sample/Spring/kyh_Point/C3/testResult.JPG)  

- 위를 해결하기 위해선 두가지 방법이 있습니다.
- 직접 자식 빈 이름을 명시해주거나, 자식 빈의 타입이 하나일 경우 그 타입으로 조회하는 방법입니다.(안좋음)

```java
    @Test
    @DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 빈 이름을 지정하면 된다")
    void findBeanByChildName(){
        DiscountPolicy rateDiscountPolicy = ac.getBean("rateDiscountPolicy", DiscountPolicy.class);
        assertThat(rateDiscountPolicy).isInstanceOf(RateDiscountPolicy.class);
    }
```
![](/assets/img/sample/Spring/kyh_Point/C3/testResult2.JPG)  

```java
//안좋은 방법
@Test
@DisplayName("특정 하위 타입으로 조회")
void findBeanByChildType(){
    RateDiscountPolicy bean = ac.getBean(RateDiscountPolicy.class);
    assertThat(bean).isInstanceOf(RateDiscountPolicy.class);

}
```
![](/assets/img/sample/Spring/kyh_Point/C3/testResult3.JPG)  

- 부모 타입으로 조회하기, 부모타입으로 조회하기 (object)

```java
    @Test
    @DisplayName("부모 타입으로 조회하기")
    void findBeanByParentType(){
        Map<String, DiscountPolicy> beansOfType = ac.getBeansOfType(DiscountPolicy.class);
        assertThat(beansOfType.size()).isEqualTo(2);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + " value = " + beansOfType.get(key));
        }
    }

    @Test
    @DisplayName("부모 타입으로 조회하기 object")
    void findBeanByParentObject(){
        Map<String, Object> beansOfType = ac.getBeansOfType(Object.class);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + " value = " + beansOfType.get(key));
        }
    }
```

## **6. Bean Factory 와 ApplicationContext

**Bean Factory**

- Bean Factory는 스프링 컨테이너의 최상위 인터페이스 입니다.
- 스프링 빈을 관리하고 조회하는 역할을 합니다.(getBean())

**ApplicationContext**

- Bean Factory 기능을 모두 상속 받아서 사용합니다.
- 빈을 관리하고 검색하는 Bean Factory 기능 뿐만아니라 수 많은 부가기능을 갖고 있습니다.
    + 메시지 소스, 환경변수, 애플리케이션 이벤트, 편리한 리소스 조회 등 다양한 기능을 포함하고 있습니다.

<br>

Bean Factory를 직접 조회해서 사용할 일이 거의 없기에 부가 기능이 포함된 ApplicationContext를 사용합니다.  
따라서 Bean Factory나 ApplicationContext를 스프링 컨테이너라고 불립니다.


-----  

## **7. 다양한 설정 형식 지원 - 자바 코드, XML** ##

**애노테이션 기반 자바 코드 설정**

지금까지 했던 것들 입니다. 과거에는 XML을 사용했습니다.

**XML 설정**

최근에는 잘 사용하지 않지만 레거시 프로젝트에서 많이 사용하고 있기에 알아두어야 합니다.  
또 XML을 사용하면 컴파일 없이 빈 설정 정보를 변경할 수 있다는 이점이 있습니다.  

'GenericXmlApplicationContext'를 사용하면서 'xml'설정 파일을 넘기면 됩니다.  

테스트를 위해 'XmlAppContext' test파일을 만듭니다.

```java
package hello.core.bean;

import hello.core.member.MemberService;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericApplicationContext;

public class XmlAppContext {

    @Test
    void xmlAppContext(){
        ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");
        MemberService memberService = ac.getBean("memberService",MemberService.class);
        Assertions.assertThat(memberService).isInstanceOf(MemberService.class);
    }
}

```

설정을 위한 appConfig.xml 파일은 메인에서 resources에 만듭니다.

> 테스트에서는 테스트 안에 넣어도 상관 없다고 하십니다.

appConfig.xml을 작성하다가 저의 memberServiceImpl 클래스가 memberServcieImpl로 오타가 나 있었습니다.. 수정했습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="memberService" class="hello.core.member.MemberServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository" />
    </bean>
    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository" />
    <bean id="orderService" class="hello.core.order.OrderServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository" />
        <constructor-arg name="discountPolicy" ref="discountPolicy" />
    </bean>
    <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy" />
</beans>
```

![](/assets/img/sample/Spring/kyh_Point/C3/xmlresult.JPG) 
![](/assets/img/sample/Spring/kyh_Point/C3/xmlresult2.JPG) 

자바 설정과 거의 비슷하지만, 하드 코딩해야하는 단점이 있는 것 같습니다.  그래도 해석할 수 있게하자면
- constructor-arg -> 생성자 name 이름 ref 레퍼런스 값
- id는 찾을 때 사용할 이름, class는 파일경로입니다.

-----  


## **8. 스프링 빈 설정 메타 정보 - BeanDefinition** ##

- 스프링의 중심에는 'BeanDefintion'이라는 추상화가 있습니다.  
    + XML을 읽어서 BeanDefinition을 만든다.
    + java 코드를 읽어서 BeanDefinition을 만든다.
    + 즉 스프링 컨테이너는 뭐가 되었든 BeanDefintion만 알면 됩니다. 이것도 역할과 구현을 나눈 것입니다.
    + BeanDefintion을 빈 설정 메타정보라고 합니다.

- 'AnnotationConfigApplicationContext' 는 'AnnotatedBeanDefinitionReader' 를 사용해서 AppConfig.class 를 읽고 BeanDefinition 을 생성합니다.
- 'GenericXmlApplicationContext' 는 'XmlBeanDefinitionReader' 를 사용해서 appConfig.xml 설정정보를 읽고 BeanDefinition 을 생성합니다.
- 새로운 형식의 설정 정보가 추가되면, XxxBeanDefinitionReader를 만들어서 BeanDefinition 을 생성하면 됩니다.

- 예제 코드를 작성하기 위해 BeanDefinitionTest.java 파일을 만듭니다.먼저 자바설정 파일을 읽었을 때입니다.  

```java
package hello.core.bean;

import hello.core.Appconfig;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class BeanDefinitionTest {

    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);

    @Test
    @DisplayName("빈 설정 메타정보 확인")
    void findApplicationBean(){
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

            //우리가 설정한 빈들만
            if(beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION){
                System.out.println("beanDefinitionName" + beanDefinitionName + "beanDefinition = " + beanDefinition);
            }
        }
    }

}

```

![](/assets/img/sample/Spring/kyh_Point/C3/metaresult.JPG)  

- XML파일을 읽었을 때 입니다.

```java
    //AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(Appconfig.class);
    GenericXmlApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");
    //로 수정해줍시다.
```

![](/assets/img/sample/Spring/kyh_Point/C3/metaresult2.JPG)  

출력 결과를 보면 둘의 출력결과가 다른 것을 확인할 수 있는데, 자바 설정파일로 할 경우 Bean Factory라는 것을 거쳐 간다고 합니다.  

**참고 BeanDefinition정보**

- BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
- factoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig
- factoryMethodName: 빈을 생성할 팩토리 메서드 지정, 예) memberService
- Scope: 싱글톤(기본값)
- lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한
- 생성을 지연처리 하는지 여부
- InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)


-----  
