---
title: Mockito doReturn vs thenReturn 설명
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-10-04 00:02:00 +0800
categories: [Java,Spring_test]
tags: [Spring]
math: true
mermaid: true
image: 
    path: 
comments: true
---

## **개요**

개인 프로젝트에서 테스트로 `Mockito`를 사용하고 있다.

처음에 `doReturn-when`을 사용하여 테스트코드를 작성하였으나, 테스트코드에서 오류가 발생했고 스택오버플로우 등에서 `doReturn-when`보다 `when-thenReturn`구문을 많이써서 그 차이를 적어보려고 한다.

처음에는 이해할 수 없었다. when-thenReturn이 doReturn.when~보다는 한국인이 읽기에는 더 쉽지 않은가? 하지만 때때로 doReturn을 써야할 때가 있다고 한다

-----

## **No type safety**

**doReturn-when**은 컴파일 단계에서 타입을 체크하지 않는다. 그래서 타입이 미스매칭 되어도 컴파일러는 잡아주지 않는다.

![](/assets/img/realworld/doReturn_thenReturn.png)

기존 `userRepository.save()`는 `User`객체를 반환하게 설계되어있다.

**doReturn-when**경우는 컴파일러가 타입체크를 하지 않아서 '123'을 반환해도 실행할 수 있게 하지만 **when-thenReturn**은 그렇지 않다.

이게 가능한한 **when-thenReturn**을 써야하는 이유 중 하나다.

-----

## **No side effect**

**doReturn-when**은 side-effect가 없다. 여기서 말하는 side-effect는 실제 함수가 호출이 되는 경우를 말한다.

그러면 **when-thenReturn**은 실제로 함수가 호출될 때가 있다는 것인데..

보통 Mockito 오브젝트를 모킹할때 제일 먼저 만든다.

```java
User user = Mockito.mock(User.class);
```

이렇게 `Mockito.mock`으로 객체를 만들었을때는 위에서 말한 **type-safety**를 제외하고는 거의 동일하게 작동한다.

하지만 `Mockito.spy`로 객체를 만들면 두 방식의 차이점이 생긴다.

Spy오브젝트는 실제 메소드와 연결을 하고 프로그래머가 이를 호출하면 실제 메소드가 호출된다. 문제는 **when-thenReturn**을 사용했을때도 실제 메소드가 호출된다는 점이다. 

이는 함수 파라미터 때문이라고 한다. **when-thenReturn**문법을 보면 실제 함수를 호출하는 부분이 있다.

```java
    @Test
    void test(){
        List list = new LinkedList();
        List spy = spy(list);

        //error
        when(spy.get(0)).thenReturn("foo");

        // You have to use doReturn() for stubbing
        doReturn("foo").when(spy).get(0);
    }
```

위의 경우라면 **when-thenReturn**은 IndexOut관련 Exception이 떠야할 것이다. 왜냐하면 빈 리스트에 `get()`함수를 실제로 호출했기 때문이다.

![](/assets/img/realworld/reallyAct.png)

실제로 에러가 발생한다.

이러한 경우 외에도 어떠한 로직에서 netowrk와 통신해야하는 로직과 실제 함수를 호출해야하는 로직이 섞여 있다면 어떻게 해야할까?

실제 함수를 호출해야하기에 `spy`를 사용해야할 것이지만 network에 의존적이지 않은 테스트코드를 짜기 위해서는 network부분을 모킹해줘야할 것이다.

그래서 **doReturn-when**을 사용하면 network부분의 실제함수를 호출하지 않기에 이를 사용해서 테스트코드를 작성할 수 있다.

## 결론

위와같은 고려사항이 없다면 **when-thenReturn**을 사용해도된다.
spy오브젝트와 함께 드물게 사용하는 **doReturn-when**도 좋지만 컴파일러에서 컴파일 에러라는 좋은 것을 발생시켜주고 가독성이 더 좋은 **when-thenReturn**이 더 활용도가 높지 않을까싶다.



## Reference

- <http://sangsoonam.github.io/2019/02/04/mockito-doreturn-vs-thenreturn.html> 원본글