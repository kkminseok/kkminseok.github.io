---
title: Spring Security - mvcMatchers vs antMatchers
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-10-11 00:02:00 +0800
categories: [Java,Spring]
tags: [Spring]
math: true
mermaid: true
image: 
    path: 
comments: true
---

## **개요**

프로젝트 코드를 복기하던 중 이런 코드를봤다.

```java
// TODO
// how diff antMatchers, mvcMatchers
@Bean
public WebSecurityCustomizer configure() throws Exception{
    return (web) -> web.ignoring().mvcMatchers("/h2-console/**");
}
```

`mvcMatchers`말고 `antMatchers`를 적어도 된다. 그 둘의 차이점이 궁금해졌다.

## **차이**

`antMatcher`는 AntPattern을 사용하는 url을 말하기에 AntPattern에 대해 알아야한다.

AntPattern에서는 3가지 특수기호가 존재하고 이를 조합해서 url을 구성한 것을 말한다.

그렇다해서 mvcPattern이 이와같은 문법을 사용하지 않는다는 것은 아니다.

- ? : 1개이상의 문자와 매칭
- ＊ : 0개 이상의 문자와 매칭
- ** : 0개 이상의 경로 또는 파일과 매칭

AntPattern은 뒤에 `슬래시(/)`가 오면 인식을 못하는데 mvcPattern는 이를 감지한다.

더 나아가 /~.html, /~.xyz처럼 뒤에 파일명이 붙어도 인식한다.

그래서 설정해줄때 mvcMatchers가 좀 더 간편하게 조작이 가능하고, 보안적으로도 우수하다고 한다.

참고로 두 개 모두 이러한 문법의 형태도 허용한다. Controller에 해당 url에 `@PathVariable`이 적용되어있어야한다.

```java
/api/user/{num}
/api/user/{*num} //매칭되어도되고 안 되어도됨.
```
## **결론**

결론적으로 `mvcMatchers`가 보안적으로 우수하고 유연하게 대처가 가능하다니 이것을 고려해보도록 하자.


## **여담**

좀 더 깊게 찾아보고 싶었으나.. 회사 점심시간 30분동안 구글링해도 외의 내용을 찾기는 어려웠다.

엄청 깊게 가면 `HandlerMappingIntrospector`와 뭔가 관련이 있는것 같은데.. 영어의 한계를 느껴서 제대로 찾진 못했다. 궁금한사람은 위의 키워드로 Spring Security 내부동작에 대해 학습하고 나한테도 알려줬으면 좋겠다.


## **Reference**

- <https://stackoverflow.com/questions/50536292/difference-between-antmatcher-and-mvcmatcher> stackOverFlow 질문글
