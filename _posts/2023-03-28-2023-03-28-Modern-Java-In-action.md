---
title: 2번 읽는 Modern Java In Action - Chapter03 람다 표현식
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2023-01-27 23:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
image: 
    path: 
comments: true
---

3장은 람다에 대한 내용이다.

전에 동작 파라미터화를 통해 재사용성을 늘릴 수 있었고, 익명 클래스를 통해 로직을 더 줄일 수 있었다.

하지만 코드 자체가 깔끔하지 않았고, 이를 해결하기 위한 자바8에서 새로나온 **람다**에 대해서 학습한다.

## 🔅 람다의 특징

- 익명: 보통의 메서드와 달리 이름이 없어서 **익명**의 특성을 가진다.
- 함수: 특정 클래스에 종속되지 않아서 **함수**라고 한다.
- 전달: 람다 표현식을 메서드 인수로 전달하거나 변수로 저장이 가능하다.
- 간결성: 익명클래스처럼 너무 많은 코드를 작성하지 않아도 된다.

자바의 기본 문법은 다음과 같다.

```text
(parameters) -> expression
(parameters) -> { statements; }
```

람다는 **함수형 인터페이스**를 통해 인자를 넘길 수 있다. 함수형 인터페이스는 단 하나의 추상 메서드를 가지는 인터페이스인데, 람다의 표현식으로 함수형 인터페이스의 추상 메서드 구현을 직접 전달할 수 있어서 전체 표현식을 함수형 인터페이스의 인스턴스로 취급이 가능하다고 한다. 

밑에는 람다와 익명 클래스, 람다표현식을 함수 파라미터에 전달해서 표현하는 코드이다.

```java
@Test
void 람다_튜토리얼(){
    //람다
    Runnable r1 = () -> System.out.println("람다 튜토리얼");

    //익명 클래스
    Runnable r2 = new Runnable() {
        @Override
        public void run() {
            System.out.println("익명 클래스");
        }
    };
    process(r1);
    process(r2);
    process(() -> System.out.println("람다 표현식 전달."));
}

private void process(Runnable r){
    r.run();
}
```

## 🔅 함수 디스크립터

람다의 표현식은 파라미터와 리턴을 가질 수 있는데, 리턴을 가질 수도 있고 안 가질수도 있고 조합에 따라 다양한 람다 표현식들이 생겨날 것이다.

다양한 람다 표현식을 구별하기 위해서는 **시그니처**가 필요한데, 람다 표현식의 시그니처를 서술하는 메서드를 **함수 디스크립터**라고 부른다.

예를 들어 `Runnable` 인터페이스의 유일한 추상 메서드 `run`메서드는 인수와 반환값이 void로 없으므로 `Runnable`인터페이스는 인수와 반환값이 없는 시그니처로 말할 수 있다.

이러한 시그니처에 따라 자바에서 제공하는 여러 함수형 인터페이스들이 있는데, 이에 대한 설명은 [여기](https://kkminseok.github.io/posts/2022-12-30-Java8Version/)에 정리해놨기에 시간 여건상 중복 내용은 따로 추가로 적지 않도록 하겠습니다.

//3.3





