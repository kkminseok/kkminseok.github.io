---
title: 2번 읽는 Modern Java In Action - Chapter04 스트림
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2023-04-13 23:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
comments: true
---

## 🔅 스트림

기존 자바7코드에서는 저칼로리의 요리명을 반환하고, 칼로리를 기준으로 요리를 정렬하려면 어떤식으로 코드를 작성했는가

```java
//Dish.java
public class Dish {
    private String name;
    private int calories;

    public Dish(String name, int calories){
        this.name = name;
        this.calories = calories;
    }

    public String getName() {
        return name;
    }

    public int getCalories() {
        return calories;
    }

}

//Test Code

@Test
void 스트림이전(){
    List<Dish> menu = Arrays.asList(
            new Dish("chicken",200),
            new Dish("pork Chop",400),
            new Dish("pork Loin",230),
            new Dish("salad", 199)
    );

    //1. 저칼로리를 뽑아냄
    List<Dish> lowCaloricDishes =  new ArrayList<>();
    for(Dish dish: menu){
        if(dish.getCalories() < 250){
            lowCaloricDishes.add(dish);
        }
    }
    Assertions.assertEquals(lowCaloricDishes.size(), 3);

    //2. 정렬(익명클래스 사용)
    Collections.sort(lowCaloricDishes, new Comparator<Dish>() {
        @Override
        public int compare(Dish o1, Dish o2) {
            return Integer.compare(o1.getCalories(), o2.getCalories());
        }
    });

    //3. 이름으로 리스트에 넣음.
    List<String> lowCaloricDishesName = new ArrayList<>();
    for(Dish dish: lowCaloricDishes){
        lowCaloricDishesName.add(dish.getName());
    }
    lowCaloricDishesName.forEach(food-> System.out.println(food));
}
```

`lowCaloricDishes`라는 사실상 결과값에 포함되지 않는 가비지 변수를 사용했다.

**자바8**에서 제공하는 코드를 사용하면 위와같은 중간변수하는 것들은 전부 라이브러리내에서 처리하게 된다.

```java
//java8 코드
@Test
void 스트림사용(){
    List<Dish> menu = Arrays.asList(
            new Dish("chicken",200),
            new Dish("pork Chop",400),
            new Dish("pork Loin",230),
            new Dish("salad", 199)
    );

    List<String> lowCaloricDishesName =
            menu.stream()
                    .filter(d -> d.getCalories() < 250)
                    .sorted(Comparator.comparing(Dish::getCalories))
                    .map(Dish::getName)
                    .collect(Collectors.toList());

    //병렬로 실행
    List<String> ParallelLowCaloricDishesName =
            menu.parallelStream()
                    .filter(d -> d.getCalories() < 250)
                    .sorted(Comparator.comparing(Dish::getCalories))
                    .map(Dish::getName)
                    .collect(Collectors.toList());

    lowCaloricDishesName.forEach(food-> System.out.println(food));
    ParallelLowCaloricDishesName.forEach(food-> System.out.println(food));
}
```

코드 수를 줄여 가독성을 높이고, 불필요한 변수를 선언할 필요가 사라졌다.

병렬처리에 대해서는 7장에서 배운다고 하니 넘어가도록 하겠다.

### 이점

- 선언형 코드로 구현이 가능하다. `if`문을 사용하여 어떻게 동작을 구현할지 고민을 할 필요 없이 정해진 연산들을 호출하여 '저칼로리의 요리만 선택하라' 라는 동작의 수행을 지정할 수 있다. 
- `filter`, `sorted`, `map` .. 같은 여러 연산들을 연결하여 파이프라인으로 만들어서 처리가 가능한데, 이를 통해 **가독성**과 **명확성**이 유지된다. 
- 병렬화를 통해 대규모 데이터처리에 능하게 대처가 가능하다.

### 정의

스트림이란 **데이터 처리 연산을 지원하도록 소스에서 추출된 연속된 요소**라고 책에서 정의하고 있다.

- 연속된 요소: 기존 컬렉션과 마찬가지로 특정 요소 형식으로 이루어진 연속된 값의 집합이라고 한다. 컬렉션의 초점은 자료구조(LinkedList, ArrayList을 사용할 것인지)에 초점이 맞춰져있고, 스트림은 filter, sorted처럼 계산에 초점이 맞춰져있다.

- 소스: 컬렉션, 배열, I/O자원 등의 데이터 제공 소스로부터 데이터를 소비한다.

- 데이터 처리 연산: 앞에서 말한 filter, map, reduce같이 데이터를 조작할 수 있게 도와주는 연산들을 말한다. 이는 순차적, 병렬적으로 실행이 가능하다.

### 특징

- 파이프라이닝: 스트림 연산끼리 연결해서 파이프라인을 구성할 수 있다. 이 덕분에 **게으름**, **쇼트서킷**같은 최적화를 얻을 수 있다.

- 내부 반복: 반복자를 명시하는 컬렉션과 달리 스트림은 내부 반복을 지원한다.



```java
@Test
void stream_tutorial_test(){
    List<String> threeHighCaloricDishNames =
            menu.stream() // 스트림 얻기
                    .filter(dish -> dish.getCalories() > 250) // 고칼로리 요리 필터링
                    .map(Dish::getName) // 요리명 추출
                    .limit(3) // 선착순 세 개만 선택
                    .collect(Collectors.toList()); // 결과를 다른 리스트로 저장
    threeHighCaloricDishNames.forEach(dishName -> System.out.println(dishName));

}
```

- 데이터 소스: 요리(리스트) 코드상에서는 `menu`가 되겠다. 연속된 요소를 스트림에 제공하고 있다.
- 데이터 처리 연산: `filter`, `map`, `limit`, `collect`이다. `collect`를 제외한 모든 연산은 서로 **파이프 라인**을 제공하고 있다.

## 🔅 스트림과 컬렉션 차이

책에서는 DVD에 대한 예시를 들고 있다. DVD에 어떤 영화가 저장되어 있다. DVD안에는 바이트나 프레임으로 이루어진 어떠한 요소들이 있을 것이다. 즉, DVD에 전체 자료구조가 저장되어 있으므로 컬렉션이다. 

DVD와 달리 인터넷 스트리밍의 경우 사용자가 시청하는 부분의 몇 프레임을 미리 내려받고, 이를 재생한다. 

DVD를 실행했을때는 사용자가 이를 다운받을 수 있는 충분한 메모리가 없을 수도 있고, 모든 요소를 다운로드 받은 후에 재생할 수 있다는 단점이 있다. 하지만 스트리밍 서비스는 그런게 없다.

즉, 컬렉션과 스트림의 차이는 데이터를 **언제**계산하느냐에 따라 다르다. 컬렉션은 자료구조가 포함하는 모든 값을 메모리에 저장하는 **자료구조다.** 그러므로 컬렉션은 모든 요소들이 컬렉션에 추가되고나서 계산되어야 한다. 

반면 스트림은 이론적으로는 **요청할 때만 요소를 계산**하는 자료구조이다. 때문에 스트림의 예제로 유명한 *무제한 소수*가 만들어질 수 있다는 것이다. 

그리고, 스트림은 **한 번만 탐색할 수 있다.** 즉, 한 번 탐색된 스트림의 요소는 소비된다.

```java
@Test
void 일회성스트림테스트(){
    List<String> title = Arrays.asList("Java8", "In", "Action");
    Stream<String> s = title.stream();
    s.forEach(System.out::println);
    s.forEach(System.out::println); // error
}

--결과

Java8
In
Action

stream has already been operated upon or closed
java.lang.IllegalStateException: stream has already been operated upon or closed
	at java.util.stream.AbstractPipeline.sourceStageSpliterator(AbstractPipeline.java:279)
	at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:647)
	at com.example.modernjavainaction.Chapter04.StreamTest.일회성스트림테스트(StreamTest.java:91)
	...
```

이렇듯 스트림은 단 한 번만 소비할 수 있다는 것이다.

스트림과 컬렉션의 또 다른 차이는 반복의 차이가 있다.

컬렉션 인터페이스는 사용자가 직접 요소를 반복 해야하는데(`for-each`구문 등) 이를 **외부 반복**이라고 한다.

스트림 라이브러리는 반복을 알아서 처리하고 결과 스트림값을 어딘가에 저장해주는 **내부 반복**을 사용한다. 

```java
@Test
void 스트림과컬렉션_반복차이(){
    //컬렉션 for-each
    List<String> names = new ArrayList<>();
    for(Dish dish: menu){
        names.add(dish.getName());
    }
    names.stream().forEach(System.out::println);

    //컬렉션 내부적으로 숨겨졌던 반복자를 사용한 외부 반복
    List<String> namesIter = new ArrayList<>();
    Iterator<Dish> iterator = menu.iterator();
    while(iterator.hasNext()){
        Dish dish = iterator.next();
        namesIter.add(dish.getName());
    }
    System.out.println("-------컬렉션 iterator 반복-------");
    namesIter.stream().forEach(System.out::println);

    //스트림 내부 반복
    List<String> namesStream = menu.stream()
            .map(Dish::getName)
            .collect(Collectors.toList());
    System.out.println("-------스트림 내부 반복-------");
    namesStream.stream().forEach(System.out::println);

}
```

내부반복 즉, 스트림을 사용하면 여러 조건이 붙을경우 순서를 적절히 제어하여 최적화하기가 더 쉬워진다. 또한 병렬적으로 수행이 가능하도록 만들 수 있고 어느정도 스트림에서 병렬성 관리를 해준다. 외부 반복을 사용하면 병렬성을 스스로 관리해야하는데, 이는 **동시성**문제를 `synchronized`와 같은 관리를 통해 개발자 스스로 제어해야한다는 뜻이다.

## 🔅 스트림 연산 특징

스트림 연산은 크게 두 그룹으로 나눌 수 있다.

- 중간 연산
- 최종 연산

### 중간 연산

중간 연산은 `filter`,`map`등 과 과 같이 스트림을 연결할 수 있는 연산들을 말한다. 

중간 연산의 중요한 특징은 스트림 파이프라인에 실행하기 전까지 아무 연산도 수행하지 않는다고 한다.(**lazy**)

이를 추적하기 위한 코드는 다음과 같다.

```java
@Test
void 중간연산(){
    List<String> names = menu.stream()
            .filter(dish -> {
                System.out.println("filtering " + dish.getName());
                return dish.getCalories() > 100;
            })
            .map(dish -> {
                System.out.println("mapping " + dish.getName());
                return dish.getName();
            })
            .limit(3)
            .collect(Collectors.toList());;
    System.out.println(names);
}
// 결과

filtering chicken
mapping chicken
filtering pork Chop
mapping pork Chop
filtering pork Loin
mapping pork Loin
[chicken, pork Chop, pork Loin]
```

스트림의 **lazy**한 특성 덕에 몇 가지 최적화 효과를 얻었다.

사실상 `menu`에 있는 **4가지 요소 전부 칼로리가 100을 넘지만, 처음 3개만 선택되었다.**

이는 `limit`연산 그리고 **쇼트서킷**이라고 불리는 기법 덕이다.

그리고, `filter`와 `map`은 다른 연산이지만 한 과정으로 병합 되었는데, 이를 **루프 퓨전**이라고 한다.

### 최종 연산

최종 연산은 스트림 파이프라인에서 결과를 도출한다. 보통 이 연산을 통해 `List`, `Integer`, `void` 등과 같은 스트림 이외의 결과가 반환된다.


## 🔅 정리

- 스트림은 소스에서 추출된 연속 요소이고, 데이터 처리 연산을 지원한다.
- 스트림은 내부 반복을 지원한다.
- 스트림은 중간 연산과 최종 연산으로 나눌 수 있다.
- 스트림 요소는 게으르게(**lazy**)하게 계산된다.