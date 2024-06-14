---
title: 2번 읽는 Modern Java In Action - Chapter05 스트림활용(2)
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2023-04-25 00:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
comments: true
---

## 🔅 개요

저번 내용이 너무 길어질 것 같아서 따로 작성하게 되었다.

## 🔅 리듀싱

**리듀싱 연산**은 모든 스트림 요소를 처리해서 값으로 도출하는 것이라고 한다.

예시를 보는게 빠른 것 같다.

다음은 모든 요소에 대해 값을 더해서 그 결과를 반환하는 테스트이다.

```java
@Test
@DisplayName("reduce 튜토리얼")
void testReduce(){
        List<Integer> numbers = Arrays.asList(1,2,3,4,5);

        Integer sum = numbers.stream().reduce(0, (a, b) -> a + b);
        //간단버전
        //Integer sum = numbers.stream().reduce(0,Integer::sum);

        //1 + 2 + 3 + 4 + 5
        System.out.println(sum);
}
```

이처럼 `reduce`는 2 가지 인수를 가진다.

- 초기값
- 두 요소를 조합해서 새로운 값을 만드는 `BinaryOperator<T>` 이다.

`reduce`에는 초기값 없는 경우도 오버로드 되어있다.

```java
//초기값 없는 경우
Optional<Integer> result = numbers.stream().reduce(Integer::sum);
//Optional[15]
System.out.println(result);
```

스트림에 아무 요소가 없는 경우가 있을 수 있으므로 `Optional`을 반환하게 된다.

### 최댓값 최솟값

`reduce`를 사용하면 최댓값, 최솟값을 찾을 수 있다.

인자로는 위에서 말한 두 요소를 조합해서 새로운 값을 만드는 `BinaryOperator<T>`형태의 값을 넣으면 된다.

```java
@Test
@DisplayName("reduce max min")
void testReduceMaxMin(){
    Optional<Integer> max = numbers.stream().reduce(Integer::max);
    Assertions.assertEquals(max.get(), 5);

    Optional<Integer> min = numbers.stream().reduce(Integer::min);
    Assertions.assertEquals(min.get(), 1);

    Optional<Integer> min2 = numbers.stream().reduce((x, y) -> x < y ? x : y);
    Assertions.assertEquals(min2.get(), 1);
}
```

이런식으로 넘기면 된다.

## 🔅 숫자형 스트림

```java
int calories = menu.stream()
    .map(Dish::getCalories)
    .reduce(0,Integer::sum);
```

이런 코드가 있는데, 위에는 **박싱 비용**이 숨겨져 있다. 내부적으로 합계를 계산하기 전에 `Integer`를 기본형(`int`)으로 언박싱해야 한다.

그리고, `reduce`연산 말고 `sum()`하나로 합계를 구할 수 있다면 더 편할 것 같다.

```java
int calories = menu.stream()
    .map(Dish::getCalories)
    .sum()
```

하지만 이는 불가능하다. `map()`의 반환형이 `Stream<T>`이기 때문에 해당 인터페이스가 `sum`을 가질 수 없기 때문이다.

이는 비효율적인데, 스트림 API 숫자 스트림을 효율적으로 처리할 수 있도록 **기본형 특화 스트림**을 제공한다.

### 기본형 특화 스트림

자바8에서는 스트림 API는 박싱 비용을 피할 수 있도록 `IntStream`, `DoubleStream`, `LongStream`을 제공한다.

위의 스트림들은 각각 숫자 스트림의 합계를 계산하는 `sum`, 최댓값을 구하는 `max`와 같은 연산 수행 메서드도 제공한다.

```java
@Test
@DisplayName("숫자 스트림 매핑")
void testMapToInt(){
    int calories = menu.stream() // Stream<Dish>
            .mapToInt(Dish::getCalories) // IntStream으로 반환
            .sum();
    System.out.println(calories);
}
```

위의 `mapToInt`는 `IntStream`을 반환한다. 이 인터페이스가 제공하는 `sum`을 통하여 합계를 계산할 수 있게 되었다.

참고로 스트림이 비어있다면 `sum`은 기본값 0을 반환한다. 외에도 `max`, `min`, `average` 등 다양한 유틸리티 메서드도 지원한다.

### 객체 스트림으로 복원

`IntStream`을 그냥 `Stream`을 바꿀 수 있을까에 대한 내용이다.

간단하게 `boxed`라는 메서드를 사용하면 된다.

```java
@Test
@DisplayName("boxed 메서드")
void testBoxed(){
    IntStream intStream = menu.stream()
            .mapToInt(Dish::getCalories);
    Stream<Integer> boxed = intStream.boxed();
}
```

### 기본값들

기본형 특화 스트림은 `max`메서드도 제공하는데, 스트림이 비어있는 경우 이를 어떻게 판단할까?

`IntStream`기준으로 이를 담기 위한 `OptionalInt`라는 자료형을 제공한다.

```java
@Test
@DisplayName("OptionalInt")
void testOptionalInt(){
    OptionalInt maxCalories = menu.stream()
            .mapToInt(Dish::getCalories)
            .max();

    System.out.println(maxCalories.orElse(1));
}
```

`OptionalInt`자료형을 통해 최댓값이 없는 상황에 사용할 기본값을 명시적으로 정의가 가능하다.

> 사실 왜 `Optional`을 사용하지 않았을까 생각하였는데, 가독성면에서 좀 더 특화된 `OptionalInt`를 사용하는게 낫겠다 생각하였다.
{: .prompt-info}

### 숫자 범위

특정 범위의 숫자를 이용해야 하는 상황이 있는데, 이를 지원한다.

`range`와 `rangeClosed`라는 두 가지 정적 메서드가 있는데, 첫 번째 인수로는 시작값을 두 번째 인수로는 종료값을 갖는다.

`range`는 시작값과 종료값이 결과에 포함되지 않고, `rangeClosed`는 포함된다.

```java
@Test
@DisplayName("숫자범위")
void testRange() {
    IntStream evenNumbers = IntStream.rangeClosed(1, 100)
            .filter(n -> n % 2 == 0);
    IntStream evenNumbers2 = IntStream.range(1, 100)
            .filter(n -> n % 2 == 0);
    Assertions.assertEquals(evenNumbers.count(), 50);
    Assertions.assertEquals(evenNumbers2.count(), 49);
}
```

## 🔅 스트림 만들기

다양한 방식으로 스트림을 만들 수 있다고 한다.

```java
//값으로 스트림 만들기
@Test
@DisplayName("값으로 스트림 만들기")
void testStreamByValue(){
    Stream<String> stream = Stream.of("Modern", "Java", "In", "Action");
    stream.map(String::toUpperCase)
            .forEach(System.out::println);
}
```

```java
//널값을 가지는 스트림 만들기
Stream<String> homeValueStream = Stream.ofNullable(System.getProperty("home"));

```

자고로 `System.getProperty()`는 key값으로 들어온 인자에 해당하는 값이 없으면 `null`을 반환한다. 이를 넣을 수 있다. **Java 9**부터다.

```java
//배열로 스트림 만들기
@Test
@DisplayName("배열로 스트림 만들기")
void testArrayStream(){
    int[] numbers = {2,3,5,7,11,13};
    int sum = Arrays.stream(numbers).sum();
    System.out.println("sum : " + sum);
}
```

```java
//무한스트림 만들기 iterate활용
@Test
@DisplayName("무한 스트림")
void testInfiniteStream(){
    //iterate활용
    Stream.iterate(0, n -> n +2)
            .limit(10)
            .forEach(System.out::println);
    //0 2 4 6 8 10 ...18
}
```

`iterate()`메서드는 첫 번째 인자로는 초기값을 가지고, 두 번째 인자로는 람다(`UnaryOperator<T>`)를 가져 새로운 값을 끊임없이 생산한다고 한다. 이러한 스트림을 **무한 스트림**이라하며 동일하게 **언바운드 스트림**이라고 한다.

보통 연속된 일련 값을 만들때는 `iterate()`를 사용한다고 한다.

```java
//무한 스트림 만들기 generate 활용
@Test
@DisplayName("generate활용")
void testGenerateStream(){
    //generate활용
    Stream.generate(Math::random)
            .limit(5)
            .forEach(System.out::println);
}
```

`generate()`는 앞에서 설명한 `iterate()`와 다르게 **계산**을 하지 않는다. 

이 두 메서드의 차이점은 책예제로는 피보나치를 구현하면 알 수 있다고 한다.

```java
//iterate로 피보나치 구현
Stream.iterate(new int[]{0,1},t -> new int[]{t[1], t[0] + t[1]} )
        .limit(20)
        .forEach(t -> System.out.println("(" + t[0] + "," + t[1] + ")"));

//generate로 구현
IntSupplier fib = new IntSupplier() {
    private int previous = 0;
    private int current = 1;

    @Override
    public int getAsInt() {
        int oldPrevious = this.previous;
        int nextValue = this.previous + this.current;
        this.previous = this.current;
        this.current = nextValue;
        return oldPrevious;
    }
};
IntStream.generate(fib)
        .limit(10)
        .forEach(System.out::println);
```

`generate`의 인자는 **상태**를 가질 수 있다. 

위 코드를 보면 `IntSupplier`의 상태를 갱신하고 있다. 즉, `getAsInt()`를 호출하면서 자신의 상태를 바꾸며 값을 생산하고 있다. 반면 `iterate`는 값은 새로 생성하지만 기존 상태를 바꾸지 않으려는 **불변** 상태를 유지하려고 한다.

근데 **병렬처리**에 있어서는 **불변함**을 고수해야한다는 것을 강조하는데, `generate`는 `getAsInt()`를 람다로 표현하지 않으면 개발자가 커스텀하여, 불변함을 유지할 수 없다. 때문에 사용하는데 주의가 더 필요한 것은 사실인 것 같다.

