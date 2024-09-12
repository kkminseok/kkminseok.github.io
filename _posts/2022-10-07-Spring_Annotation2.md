---
title: Spring - @SpringbootApplication가 뭘까 2편 EnableAutoConfiguration
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-10-06 00:02:00 +0800
categories: [Java,Spring]
tags: [Spring]
math: true
mermaid: true
comments: true
---

## **개요**

저번 포스팅에서는 메타 어노테이션들을 봤다. 이번에는 `@EnableAutoConfiguration`과 `@ComponentScane` 속성값들을 파볼것이다.

## **@EnableAutoConfiguration**

이 또한 Javadoc를 읽어보면 친절하게 알려준다.

간단하게 이 어노테이션은 Bean을 등록하는 설정파일이고, spring.factories 내부에 따라 여러 Configure들이 있고 이를 조건에따라 등록하는 것이다.

`@SpringBootApplication`안에 있는 이 어노테이션이 결국 실행될 때 spring.factories에 있는 수많은 설정들이 자동으로 조건에 따라 적용되어 Bean이 생기는 것이다.

![](/assets/img/realworld/springbootapplication/auto.png)

해당 jar파일을 보면 `spring.factories`가 있는걸 볼 수 있다. 기본적인 class-path에 저장된 값들도 등록한다.

예제는 [이블로그](https://velog.io/@max9106/Spring-Boot-EnableAutoConfiguration)에 잘 나와있다.

또한 이를 확인하는 방법은 간단한데, `@EnableAutoConfiguration`어노테이션에 지정되어있는 `@Import(AutoConfigurationImportSelector.class)`의 AutoConfigurationImportSelector는 
DeferredImportSelector, BeanClassLoaderAware, ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered를 구현하게 되어있는데 이것들은 이름에서도 알 수 있게 특정 조건에 따라서 빈들을 담아온다.

그래서 이 들 중 하나에 들어가서 디버그를 찍으면 빈들이 등록되는것을 볼 수 있다.

```text
@EnableAutoConfiguration
    -> @AutoConfigurationPackage
        -> @Import(AutoConfigurationPackages.Registrar.class) 에서 Registrar
```

를 타고 들어가면 다음과 같이 구현 되어있는걸 볼 수 있다.

```java
static class Registrar implements ImportBeanDefinitionRegistrar, DeterminableImports {

    @Override
    public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
        register(registry, new PackageImports(metadata).getPackageNames().toArray(new String[0]));
    }

    @Override
    public Set<Object> determineImports(AnnotationMetadata metadata) {
        return Collections.singleton(new PackageImports(metadata));
    }

}
```

여기서 `register()`의 인자를 설명해보면, `registry`는 빈 정의에 대해 관리를 하고, 두번째 인자로 넘겨주는 것은 이 어플리케이션에 있는 패키지 이름들이다.

디버그를 찍고 확인해보면

![](/assets/img/realworld/springbootapplication/registy.png)

`beanDefinitionNames`에 내 프로젝트에 등록된 빈 이름들을 볼 수 있다.

이는 근데 어떻게 미리 알고 있을까? 이는 뒤에나올 ComponentScan을 먼저 수행하기 때문이다.

아무튼 빈에 대한 정보들을 가지고 있고 `new PackageImports~`이부분은 내 프로젝트 같은 경우 이렇게 이루어져있다.

![](/assets/img/realworld/springbootapplication/packagenames.png)

실제 디버그를 또 직고 구현과정을 보면 `basePackageClasses`이라는 것과 `basePackages`라는 것을 넣고있는데,

이 두개는 다음과 같이 활용되기도 한다.

```java
@ComponentScan(basePackageClasses = Application.class)
@ComponentScan(basePackages = com.io.realworld)
```

별 다른 조건이 없으면 **@SpringBootApplication**가 정의된 곳이 basePackage가 되는것이다.

이 어노테이션은 `@Import(AutoConfigurationImportSelector.class)`라는 구문도 가지고 있는데

위에서 설명한 `META-INF/spring.factories`에 있는 자동설정해야할 클래스들의 이름 목록을 반환한다. 이 또한 디버그를 찍고 보면

![](/assets/img/realworld/springbootapplication/factories.png)

내가 안 쓴 Configuration이 있는거 보니 기본적인 Configuration은 다 가져온다.

## **ComponentScan**

컴포넌트 스캔은 `@Configuration`, `@Controller`, `@Service`, 등 다른 `@Component`가 붙은 클래스들을 스캔하고 빈에 등록한다.

위에서 `basePackageClasses`, `basePackages`에 대해서 말했는데 이를 따로 설정해주지 않으면 디폴트 설정 패키지를 스캔한다.

```java
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class)
```

구문을 해석하자면 Custom타입의 필터를 등록하는데, TypeExcludeFilter 클래스에 정의된 필터로 가져온다는 뜻이다.

그러면 `TypeExcludeFilter`에 있는 `match()`메소드를 봐야한다. 왜냐하면 TypeFilter를 상속받고 있는데, 이 메소드를 오버라이딩 해줘야하기 때문이다.

```java
@Override
public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory)
        throws IOException {
    if (this.beanFactory instanceof ListableBeanFactory && getClass() == TypeExcludeFilter.class) {
        for (TypeExcludeFilter delegate : getDelegates()) {
            if (delegate.match(metadataReader, metadataReaderFactory)) {
                return true;
            }
        }
    }
    return false;
}
```

`TypeExcludeFilter`클래스에 있는 메소드다. 무슨의미인지 해석하다가 날밤 샐거 같아서 나중에 기회되면 알아보는게 좋을것 같다.

또한 

```java
@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
```

이것도 적용되어있는데 비슷한 방식으로 

```java
//AutoConfigurationExcludeFilter.class
@Override
public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory)
        throws IOException {
    return isConfiguration(metadataReader) && isAutoConfiguration(metadataReader);
}
```

로 되어있다.

## **결론**

`@SpringBootApplication`은 

- `@EnableAutoConfiguration`를 통해 설정들을 자동으로 해주고
- `@ComponentScan`을 통해 각 필터에 맞는 어노테이션을 스캔해준다.

더 깊이 파고 싶지만 머리가 복잡해져간다. 점점 산으로 가는 느낌도 들고 일단 목적에 맞게 `@SpringBootApplication` 어노테이션이 어떤식으로 동작되는지 간단하게 알아봤다.


## **Reference**

- <https://www.baeldung.com/spring-componentscan-filter-type> filterType