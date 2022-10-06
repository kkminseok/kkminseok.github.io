---
title: Spring - @SpringbootApplication가 뭘까 1편 메타어노테이션까지
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-10-06 00:02:00 +0800
categories: [Java,Spring]
tags: [Spring]
math: true
mermaid: true
image: 
    path: 
comments: true
---

## **개요**

`@SpringbootApplication`에 대해 설명해 달라는 질문을 받았다.

당연히 main에 붙어있고 그 이후에 볼 일이 없어서 당연시하게 쳐다도 안 보게 되었다.(사실 의식하지 않으면 있는지도 몰랐음.)

boot를 쓴다하면 다 붙어있는데 답변을 못했고 부끄러워서 정리를 해보려고 한다.

## **뜻**

부트의 가장 기본적인 설정을 해준다고 알려져 있다.

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
    ...
}
```
으로 되어있는데 어노테이션 하나씩 까보도록 하자.

## @Target(ElementType.TYPE)

Target 어노테이션은 이 어노테이션이 붙을 수 있는 타입에 대해 지정한다.

![](/assets/img/realworld/springbootapplication/targetType.png)

위 표를 보면 **Type**은 **Class, Interface, enumeration**에 붙을 수 있다고 써있다.

즉, `@SpringBootApplication`어노테이션은 우리가 프로젝트로 생성한 main을 감싸고 있는 클래스 위에 붙어있으므로 맞게 부착되었다고 할 수 있다.

## @Retention(RetentionPolicy.RUNTIME)

Retention의 소스를 까보면

```java
Indicates how long annotations with the annotated interface are to be retained. If no Retention annotation is present on an annotation interface declaration, the retention policy defaults to RetentionPolicy.CLASS.
A Retention meta-annotation has effect only if the meta-annotated interface is used directly for annotation. It has no effect if the meta-annotated interface is used as a member interface in another annotation interface.
Since:
1.5
Author:
Joshua Bloch
jls
9.6.4.2 @Retention

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
    /**
     * Returns the retention policy.
     * @return the retention policy
     */
    RetentionPolicy value();
}
```

이렇게 되어있다. Target이 `ANNOTATION_TYPE`으로 되어있어서 결국 `@Retention`은 어노테이션에만 붙을 수 있다. 

윗 주석을 읽어보면 어노테이션의 설명을 볼 수 있다. 어노테이션을 어디까지 살려둘 지를 결정하며 
디폴트값은 CLASS이다. `RetentionPolicy`값을 넘기면서 어노테이션 메모리 범위를 정해주는 것인데, 이는 `enum`으로 되어 있다.

```java
// RetentionPolicy enum

public enum RetentionPolicy {
    /**
     * Annotations are to be discarded by the compiler.
     */
    SOURCE,

    /**
     * Annotations are to be recorded in the class file by the compiler
     * but need not be retained by the VM at run time.  This is the default
     * behavior.
     */
    CLASS,

    /**
     * Annotations are to be recorded in the class file by the compiler and
     * retained by the VM at run time, so they may be read reflectively.
     *
     * @see java.lang.reflect.AnnotatedElement
     */
    RUNTIME
}

```

SOURCE, CLASS, RUNTIME을 넘길 수 있으며 주석으로 살아있는 시점이 적혀져 있다.

이를 이해하려면 JVM 동작방식에 대해 알고 있어야한다.

- SOURCE : complie될 때 어노테이션을 메모리에서 해제. 즉, 소스코드(.java)까지만 남아있는다.
- CLASS : .class파일에 어노테이션을 기록, 이후 메모리에서 해제. 즉, 런타임때는 남아있지 않음.
- RUNTIME : RUNTIME때까지 살아있는 어노테이션

<https://jeong-pro.tistory.com/234> 포스팅의 댓글을 보면 이해가 빠르다.

결론은 `@SpringBootApplication`은 `@Retention(RetentionPolicy.RUNTIME)`이므로 런타임때까지 살아있는다. 

> 개인적인 생각입니다.
{: .prompt-warning}
개인적인 생각으로 왜 런타임때까지 살아있어야하냐면 런타임때까지 살아있으면 어노테이션의 정보를 뽑아쓸 수 있는데(Reflection) 컴포넌트 스캔은 런타임때 어노테이션의 정보를 가지고 스캔을 돌리므로 `@SpringBootApplication`도 런타임때까지는 살아있어야하는 것이라는 추측이다.

## **@Documented**

이 어노테이션은 자바doc로 API를 만들때 해당 어노테이션에 대한 설명도 추가해주는 어노테이션이다.

## **@Inherited**

상속받은 클래스에도 어노테이션 속성이 붙는다.

보통 main을 포함하는 클래스는 하나이고 이를 상속받을 일은 없기에 `@Inherited`가 붙어있는 이유를 모르겠어서 생각해봤지만.. 프로젝트가 좀 더 커지면 그런 일이 생기겠지?.. 좀 더 생각이 필요하다.

예제 : <https://hongsii.github.io/2018/12/12/java-annotation/>


여기까지는 **메타 어노테이션**이다. 그래서 해석하는데 어려움은 없었다. 

## **@SpringBootConfiguration**

이 또한 들어가보면 다음처럼 코드가 되어있다.

```java
/**
 * Indicates that a class provides Spring Boot application
 * {@link Configuration @Configuration}. Can be used as an alternative to the Spring's
 * standard {@code @Configuration} annotation so that configuration can be found
 * automatically (for example in tests).
 * <p>
 * Application should only ever include <em>one</em> {@code @SpringBootConfiguration} and
 * most idiomatic Spring Boot applications will inherit it from
 * {@code @SpringBootApplication}.
 *
 * @author Phillip Webb
 * @author Andy Wilkinson
 * @since 1.4.0
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
@Indexed
public @interface SpringBootConfiguration {

	/**
	 * Specify whether {@link Bean @Bean} methods should get proxied in order to enforce
	 * bean lifecycle behavior, e.g. to return shared singleton bean instances even in
	 * case of direct {@code @Bean} method calls in user code. This feature requires
	 * method interception, implemented through a runtime-generated CGLIB subclass which
	 * comes with limitations such as the configuration class and its methods not being
	 * allowed to declare {@code final}.
	 * <p>
	 * The default is {@code true}, allowing for 'inter-bean references' within the
	 * configuration class as well as for external calls to this configuration's
	 * {@code @Bean} methods, e.g. from another configuration class. If this is not needed
	 * since each of this particular configuration's {@code @Bean} methods is
	 * self-contained and designed as a plain factory method for container use, switch
	 * this flag to {@code false} in order to avoid CGLIB subclass processing.
	 * <p>
	 * Turning off bean method interception effectively processes {@code @Bean} methods
	 * individually like when declared on non-{@code @Configuration} classes, a.k.a.
	 * "@Bean Lite Mode" (see {@link Bean @Bean's javadoc}). It is therefore behaviorally
	 * equivalent to removing the {@code @Configuration} stereotype.
	 * @return whether to proxy {@code @Bean} methods
	 * @since 2.2
	 */
	@AliasFor(annotation = Configuration.class)
	boolean proxyBeanMethods() default true;

}
```

기본으로 제공되는 메타 어노테이션은 위에서 다 설명했고 특이한점이 없다. `@Configuration`이 붙어있는데 이는 환경설정 빈을 표현하는 어노테이션이다.

맨위의 설명 주석을 보면 **@Configuration**을 대신 쓸 수 있다고 써 있다.

그리고 그 밑에 '@SpringBootConfiguration' 어노테이션은 하나만 붙어있을 수 있고, 이는 '@SpringBootApplication'어노테이션이 상속한다고 되어있는데..

[어떤 글](https://livenow14.tistory.com/52)에서는 계층을 주기 위함이 아니냐라고 써있다.

```text
@SpringBootApplication
-------> @SpringBootConfiguration
       -------> @Configuration
```

구성을 자동으로 찾아준다는게 일반 `@Cofiguration`과 다른점이라고 한다. 이는 단위테스트나 통합테스트를 작성할때 유용하다고는 써있지만..

그 방식은 모르겠지만 일단 뒤로 넘어가면 힌트가 있을 수 있다. 그래서 다음부분을 보겠다.

는 2편에서 계속 작성..

## **여담**

오래 안 걸릴줄 알고 회사 점심시간 1시간을 할애해서 작성하였는데, 생각보다 작성해야할 내용이 많았다. 그래서 집에서 추가로 2편을 작성해야할것 같다.




## Reference

- <https://www.geeksforgeeks.org/java-target-annotations/> Target 어노테이션 타입표
- <https://jeong-pro.tistory.com/234> Retention 설명 관련글.
- <https://hongsii.github.io/2018/12/12/java-annotation/> Inherited어노테이션 예제
- <https://livenow14.tistory.com/52> Spring 설정 계층