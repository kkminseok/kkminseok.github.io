---
title: 2번 읽는 Modern Java In Action - Chapter06 스트림으로 데이터 수집
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-02 00:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
image: 
    path: 
comments: true
---

## 🔅 개요

이전 챕터에서는 `collect`메서드로 `Collector`인터페이스 구현을 전달하였는데, 해당 인터페이스는 스트림의 요소를 어떤 식으로 도출할지를 지정해준다. 

함수형 프로그래밍은 **무엇**을 원하는지 직접 명시할 수 있어서 명령형 프로그래밍과 다르게 코드가 좀 더 간결하고 가독성이 향상된다. 자바에서 제공하는 함수형 API를 살펴볼 챕터인것 같다.

컬렉터의 메소드로 스트림의 갯수를 셀 수 있으며,

```java
@Test
@DisplayName("리듀스 카운트 예제")
void test_reduce_counting() {
    long result = menu.stream().count();
    System.out.println(result);
}
```

컬렉션의 메소드로 스트림의 최댓값 최솟값을 구할 수 있다.

```java
@Test
@DisplayName("리듀스 최댓값 최솟값 예제")
void test_reduce_max_min(){
    Comparator<Dish> dishComparator = Comparator.comparingInt(Dish::getCalories);
    Optional<Dish> mostCaloriesDish = menu.stream().collect(Collectors.maxBy(dishComparator));

    Assertions.assertEquals(mostCaloriesDish.get().getCalories(), 400);
}

```

컬렉션에서 제공하는 메소드중 **요약**팩토리 메서드를 제공하는데, 이를통해 스트림의 합계나 평균값, 등을 계산할 수 있다.

```java
@Test
@DisplayName("리듀스 요약 연산 예제")
void test_reduce_summing(){
    Integer totalCalories = menu.stream().collect(Collectors.summingInt(Dish::getCalories));

    Assertions.assertEquals(totalCalories, 1029);

    double avgCalories = menu.stream().collect(Collectors.averagingInt(Dish::getCalories));

    Assertions.assertEquals((int)avgCalories, (int)totalCalories / menu.stream().count());
}

```

또한 위의 정보를 모은 `~Statistics()` 메서드를 통해 정보들을 확인할 수 있다.

```java
@Test
@DisplayName("리듀스 summary 예제")
void test_reduce_summary(){
    IntSummaryStatistics menuStatistics = menu.stream().collect(Collectors.summarizingInt(Dish::getCalories));
    //IntSummaryStatistics{count=4, sum=1029, min=199, average=257.250000, max=400}
    System.out.println(menuStatistics);
}

```

문자열도 연결할 수 있다.

```java
@Test
@DisplayName("문자열 연결 예제")
void test_join_string(){
    String shortMenu = menu.stream().map(Dish::getName).collect(Collectors.joining());
    //chickenpork Choppork Loinsalad
    System.out.println(shortMenu);

    //구분자를 넣음
    shortMenu = menu.stream().map(Dish::getName).collect(Collectors.joining(", "));
    //chicken, pork Chop, pork Loin, salad
    System.out.println(shortMenu);
}
```