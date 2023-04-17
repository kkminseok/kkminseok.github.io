---
title: 2번 읽는 Modern Java In Action - Chapter05 스트림활용(1)
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2023-04-17 00:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
image: 
    path: 
comments: true
---

## 🔅 개요

스트림 API가 지원하는 다양한 연산들을 다뤄본다고 한다.

참고로 여기서 나오는 **menu**리스트들은

```java
@BeforeEach
void init() {
    menu = Arrays.asList(
            new Dish("chicken", false, 200, Dish.Type.MEAT),
            new Dish("pork Chop", false, 400, Dish.Type.FISH),
            new Dish("pork Loin", false, 230, Dish.Type.FISH),
            new Dish("salad", true, 199, Dish.Type.OTHER)
    );
}
```

이렇게 구현되어있다.

## 🔅 필터링

- `filter`메서드는 **프레디케이트**를 인수로 받아서 해당 프레디케이트와 일치하는 모든 요소를 포함하는 스트림을 반환한다.

```java
@Test
@DisplayName("프리디게이트로 필터링")
void testPredicateFilter(){
    List<Dish> vegetarianMenu = menu.stream()
            .filter(Dish::isVegetarian)
            .collect(Collectors.toList());
    //salad
    vegetarianMenu.stream().forEach(System.out::println);
}
```

- `distinct()`를 통한 중복제거도 가능하다.

```java
@Test
@DisplayName("프리디게이트로 필터링 - 중복제거")
void testPredicateDistinctFilter() {
    List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);
    //중복제거
    //2, 4
    numbers.stream()
            .filter(i -> i % 2 == 0)
            .distinct()
            .collect(Collectors.toList())
            .forEach(System.out::println);
}
```

## 🔅 스트림 슬라이싱

만약 칼로리순으로 정렬된 경우이고, 320칼로리 이하의 요리를 선택해야한다면 어떻게 할 수 있을까

```java
List<Dish> sortedMenu = Arrays.asList(
                new Dish("fruit", true, 120, Dish.Type.OTHER),
                new Dish("prwans", false, 300, Dish.Type.FISH),
                new Dish("rice", false, 350, Dish.Type.OTHER),
                new Dish("chicken", false, 400, Dish.Type.MEAT),
                new Dish("french fries", true, 530, Dish.Type.OTHER)
        );

sortedMenu.stream()
                .filter(dish -> dish.getCalories() < 320)
                .collect(Collectors.toList());
```

이런식으로 할 수 있지만, 정렬이 되었음에도 불구하고 4번째인 400칼로리도 `filter`가 적용될 것이고 5번째인 800칼로리도 마찬가지로 돌것이다. 그렇게 무의미하게 필터가 계속 작용될 것이다.

**Java9**버전에서는 `taskWhile()`를 통해 정렬된 값에 대한 필터를 적용하여 범위를 벗어난값이 나타나면 연산을 종료시킬 수 있다.

```java
List<Dish> result = sortedMenu.stream()
        .takeWhile(dish -> dish.getCalories() < 320)
        .collect(Collectors.toList());
//fruit, prawns
result.stream().forEach(System.out::println);
```

이런식으로 해결할 수 있다.

반대로 320칼로 이상의 요소를 선택하려면 어떻게 해야할까?

```java
List<Dish> result = sortedMenu.stream()
        .dropWhile(dish -> dish.getCalories() < 320)
        .collect(Collectors.toList());

//rice, chicken, french fries
result.stream().forEach(System.out::println);
```

`dropWhile`은 프레디케이트가 처음으로 거짓이 되는 지점까지 발견된 요소를 버린다. 그리고 거짓이 되면 그 지점에서 작업을 중단하고 남은 모든 요소를 반환한다.

`limit(n)`를 이용해 스트림 최대 요소 n개를 반환할 수 있다.

```java
@Test
@DisplayName("limit를 이용한 테스트")
void testLimit(){
    List<Dish> sortedMenu = Arrays.asList(
            new Dish("fruit", true, 120, Dish.Type.OTHER),
            new Dish("prwans", false, 500, Dish.Type.FISH),
            new Dish("rice", false, 350, Dish.Type.OTHER),
            new Dish("chicken", false, 400, Dish.Type.MEAT),
            new Dish("french fries", true, 530, Dish.Type.OTHER)
    );

    List<Dish> dishes = sortedMenu.stream()
            .filter(dish -> dish.getCalories() > 300)
            .limit(3)
            .collect(Collectors.toList());
    
    // prwans
    // rice
    // chicken
    dishes.stream().forEach(System.out::println);
}
```

`skip(n)`을 통하여 스트림 요소 n개를 건너 뛸 수 있다.

```java
@BeforeEach
void init() {
    menu = Arrays.asList(
            new Dish("chicken", false, 200, Dish.Type.MEAT),
            new Dish("pork Chop", false, 400, Dish.Type.FISH),
            new Dish("pork Loin", false, 230, Dish.Type.FISH),
            new Dish("salad", true, 199, Dish.Type.OTHER)
    );
}

@Test
@DisplayName("skip을 이용한 테스트")
void testSkip(){
    List<Dish> dishes = menu.stream()
            .filter(d -> d.getCalories() < 300)
            .skip(2)
            .collect(Collectors.toList());
    //salad
    dishes.stream().forEach(System.out::println);
}
```

## 🔅 매핑

특정객체의 특정 데이터를 선택하는 작업이 필요할 때도 있을 것이다. 이는 `map`과 `flatMap`메서드가 제공한다.

```java
@Test
@DisplayName("map 테스트")
void testMap(){
    List<String> dishNames = menu.stream()
            .map(Dish::getName)
            .collect(Collectors.toList());
    // chicken
    // pork Chop
    // pork Loin
    // salad
    dishNames.stream().forEach(System.out::println);
}
```

`Dish::getName`은 문자열을 반환하므로 map 메서드의 출력 스트림은 **Stream<String>** 형식을 갖는다.

만약 각 요리명의 길이를 알고 싶으면 어떻게 할까? `map`을 중첩해서 사용하면 된다.

```java
@Test
@DisplayName("map 중첩 테스트")
void testMapDuplicate(){
    List<Integer> dishNameLengths = menu.stream()
            .map(Dish::getName)
            .map(String::length)
            .collect(Collectors.toList());
    //7 9 9 5 
    dishNameLengths.stream().forEach(System.out::println);
}
```

## 🔅 스트림 평면화

책에서 `flatMap`은 배열을 스트림이 아니라 스트림의 콘텐츠로 매핑한다는데, 이해가 잘 되지 않는다.

만약 ["Hello", "World"]를 ["H","e","l","l","o","W","o","r","l","d"]로 쪼개고 싶다고 하자.

그러면

```java
List<String> words = Arrays.asList("Hello", "World");

        //평면화 전
List<String[]> mapResult = words.stream()
        .map(word -> word.split(""))
        .collect(Collectors.toList());
```

결과는 [["H","e","l","l","o"],["W","o","r","l","d"]] 로 나올 것이다.

우리가 원하는 건 2개의 배열에 결과를 담는게 아니다.

`flatMap`은 이러한 작업을 도와주는데, 스트림을 1차원 평면화 시킨다고 보면된다.

```java
@Test
@DisplayName("스트림 평면화")
void testMapDistinct(){
List<String> words = Arrays.asList("Hello", "World");

List<String> result = words.stream()
        .map(word -> word.split(""))
        .flatMap(Arrays::stream) // 배열을 스트림으로 변환
        .collect(Collectors.toList());

result.stream().forEach(System.out::println);
}
```

## 🔅 검색과 매칭

검색하는데 용이한 메서드들을 제공한다는 것이다.

### anyMatch

프레디케이트가 주어진 스트림에서 적어도 한 요소와 일치하는지 확인할 때 사용한다.

```java
@Test
@DisplayName("anyMatch 테스트")
void testAnyMatch(){
        if(menu.stream().anyMatch(Dish::isVegetarian)){
                System.out.println("is vegetarian");
        }
}
```

### allMatch

스트림의 모든 요소가 주어진 프레디케이트와 일치하는지를 검사한다.

```java
@Test
@DisplayName("allMatch 테스트")
void testAllMatch(){
        if(menu.stream().allMatch(dish -> dish.getCalories() < 1000)){
                System.out.println("모든 음식의 칼로리가 1000보다 적습니다.");
        }
}
```

### noneMatch

스트림의 모든 요소가 주어진 프레디케이트와 일치하지 않는지를 검사한다.

```java
@Test
@DisplayName("noneMatch 테스트")
void testNoneMatch(){
        if(menu.stream().noneMatch(dish -> dish.getCalories() > 1000)){
                System.out.println("모든 음식의 칼로리가 1000을 초과하지 않습니다.");
        }
}
```

### findAny

스트림에서 임의의 요소를 반환한다. 예제에서는 자료구조의 양이 적어서 매 번 임의의 값이 나오지는 않는다.

```java
@Test
@DisplayName("findAny 테스트")
void testFindAny(){
        Optional<Dish> dish = menu.stream()
                .filter(Dish::isVegetarian)
                .findAny();
        //Optional[salad]
        System.out.println(dish);
}
```

> Optional에 관한 내용은 10챕터에 자세히 나온다니, 생략하도록 하겠다. 간단히 `null`을 처리하기 위해 등장한 클래스다.
{: .prompt-info}

### findFirst

스트림에서 첫 번째 요소를 찾기위해 사용한다.

```java
@Test
@DisplayName("findFirst 테스트")
void testFindFirst(){
        List<Integer> numbers = Arrays.asList(1,2,3,4,5);

        numbers.stream()
                .map(n -> n * n)
                .filter(n -> n % 3 == 0)
                .findFirst()
                //9출력
                .ifPresent(i -> System.out.println(i));
}
```

> 병렬 실행에서는 첫 번째 요소를 반환하는 `findFirst`를 제대로 이용하기 어렵다. 때문에 `findAny`를 사용하는게 좋다.
{: .prompt-tip}


내용이 길어서 2개로 나눠서 작성하게 되었다.