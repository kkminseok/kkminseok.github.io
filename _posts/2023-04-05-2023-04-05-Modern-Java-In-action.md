---
title: 2번 읽는 Modern Java In Action - Chapter03 람다 표현식
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2023-04-05 23:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
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

위에서 제공하는 함수형 인터페이스들은 제네릭 파라미터를 사용하기에 참조형만 사용할 수 있다. 

참조형을 기본형으로 변환하는것을 **언박싱** 하는데, 이를 자동으로 이루어지게 **오토박싱**기능도 제공하므로 프로그래머는 편리하게 코드를 작성할 수 있다.

근데 오토박싱 기능을 사용하면 변환과정에서 비용을 소모하게 되는데, 자바8에서 제공하는 함수형 인터페이스를 통해 오토박싱 동작을 피할 수 있도록 한다.

```java
@Test
void 오토박싱_x(){
    IntPredicate evenNumbers = (int i) -> i % 2 == 0;
    //박싱 x
    Assertions.assertEquals(evenNumbers.test(1000), true);

    Predicate<Integer> oddNumbers = (Integer i) -> i%2 != 0;
    //박싱 o
    Assertions.assertEquals(oddNumbers.test(1000), false);

}

@FunctionalInterface
interface IntPredicate{
    boolean test(int t);
}

```

사실 특별한건 없고 함수형 인터페이스의 인자를 제너릭 파라미터가 아닌, 기본형으로 받도록 바꿨을 뿐이다. 이를통해 `int`형이 들어오면 박싱하지 않도록 할 뿐이다.

보통 특별한 인자를 받는 경우 함수형 인터페이스 앞에 `DoublePredicate` 와 같이 자료형을 앞에 붙인다.

이외에도 여러 함수형인터페이스를 커스텀해서 만들 수 있다.

## 🔅 형식 검사, 형식 추론, 제약

람다가 어떤 방식으로 실행되는지, 어떻게 컴파일러는 이를 인식하는지 알 필요가 있다.

책에서는 

```java
List<Apple> apples = filter(inventory, (Apple apple) -> apple.getWeight() > 150);
```

이라는 코드를 가지고 설명하고 있으므로, 내 생각을 덧붙여서 정리하겠다.

1. `filter`메서드의 선언을 확인한다.
2. `filter`메서드의 두 번째 파라미터를 보고, 개발자는 함수형 인터페이스중 맞는 것(Predicate<Apple>)을 찾아 그 형식을 기대하도록 설계한다.
3. `Predicate<Apple>`는 `test`라는 한 개의 추상 메서드를 가지고 반환형은 `boolean`이므로 이 메서드를 사용할때 이러한 요구사항을 만족해야한다.

위의 식은 해당 요구사항을 만족하므로 유효한 코드이다.

```java
Comparator<Apple> c1 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
ToIntBiFunction<Apple,Apple> c2 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

위 코드는 하나의 람다 표현식을 다양한 함수형 인터페이스로 받을 수 있다.

이는 `<>`라는 컨텍스트를 통해 제너릭 형식을 추론할 수 있게 한다. 

즉, 람다는 **할당문 콘텍스트, 메서드 호출 콘텍스트(파라미터, 반환값), 형변환 콘텍스트** 등으로 람다 표현식의 형식을 추론할 수 있다.

자바 컴파일러는 위의 방식으로 함수 디스크럽터를 추론할 수 있는데, 이는 즉 람다의 시그니처도 추론할 수 있다는 것이다.

그렇기에 문법에서 생략할 수 있는 부분이 생긴다.

위의 코드에서

```java
List<Apple> apples = filter(inventory, apple -> apple.getWeight() > 150);

//또는
Comparator<Apple> c1 = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight());
```

이런식으로 생략이 가능하고 때에 따라서는 가독성을 향상시킬 수도 있다.

-----

지금까지 사용한 람다는 모두 인수를 자신의 바디 안에서만 사용했는데, 외부 변수를 가져다 쓸 수도 있다.

이를 **람다 캡쳐링**이라고 부르기도 한다.

```java
@Test
void 람다캡쳐링(){
    int pointNumber = 1553;
    Runnable r = () -> System.out.println(pointNumber);
    r.run();
}
```

이런식으로 외부 변수인 `pointNumber`를 인자로 넘겨서 사용할 수 있다.

하지만 제약이 있다.

- 넘기는 변수는 `final`로 선언되어 있거나 `final`처럼 사용되어야 한다. 즉, 한 번만 할당되어야 한다.

![image](https://user-images.githubusercontent.com/30401054/229855297-50fa6817-9ba7-4110-a7fe-8ee752276aa5.png)

> 재할당하니 컴파일 오류를 나타내는 것을 볼 수 있다.
{: .prompt-tip }

왜 이런 제약이 있냐면, 정말 간단하게 말하면 pointNumber가 1553값일때 람다식을 실행하려고 하는데, 밑에서 10000으로 재할당하는 경우가 생겨버려, 예기치 못한 에러를 발생시킬 수 있기 때문이다. 
즉, 람다식 내부에서 사용하려는 지역변수 복사본이 재할당 되는 경우가 생겨버리므로 이를 막기 위해 컴파일 에러를 발생시킨다는 것이다.

## 🔅 메서드 참조

메서드 참조를 사용하면 메서드 정의를 활용해서 람다처럼 전달할 수 있다고 한다.

```java
@Test
void 메서드참조(){
    List<Apple> inventory = Arrays.asList(
            new Apple(200, Color.RED),
            new Apple(190, Color.GREEN),
            new Apple(180, Color.RED)
    );
    //참조 사용
    inventory.sort(Comparator.comparing(Apple::getWeight));
    inventory.forEach(apple -> System.out.println(apple.getWeight()));
}
```

이처럼 `Apple::getWeight`라는 문법이 **메서드 참조**이다.

람다에게 메서드를 어떻게 호출하는지에 명시하는 것이 아닌, 메서드에 직접 참조하는 것이 가독성을 높일 때가 있다.

위의 식은 `(Apple a) -> a.getWeight()`를 축약한 것이다.

위와 비슷하게 객체를 메서드 참조를 통해 생성할 수도 있다.

```java
@Test
void 생성자참조() {
    //기본 생성자
    //Supplier<Apple> c1 = () -> new Apple();
    Supplier<Apple> c1 = Apple::new;
    Apple a1 = c1.get();

    // weight가 있는 생성자
    // Function<Integer, Apple> c2 = (weight) -> new Apple(weight);
    Function<Integer, Apple> c2 = Apple::new;
    Apple a2 = c2.apply(150);

    //생성자 1개일때
    List<Integer> weights = Arrays.asList(7, 3, 4, 10);
    List<Apple> apples = mapByNewRef(weights, Apple::new);
    Assertions.assertEquals(apples.size(),4);

    //생성자 2개일때
    BiFunction<Integer, Color, Apple> c3 = Apple::new;
    Apple a3 = c3.apply(150, Color.GREEN);
}
```

책에서는 여러 함수형 인터페이스를 조합해서 사용하는 방법도 알려주고 있다.

예를들어, `Comparator` 인터페이스에는 `reversed()`와 같이 역정렬도 가능하고, `thenComparing()`같은 메소드를 통해 어떠한 처리 이후의 비교처리를 할 수 있게 한다.

```java
inventory.sort(comparing(Apple::getWeight))
         .reversed() //무게를 내림차순 정렬
         .thenComparing(Apple::getCountry); //두 사과의 무게가 같으면 국가별 정렬
```

또한 `Predicate`인터페이스는 `negate`, `and`, `or` 세가지 메서드를 통해 사용자 요청을 조합할 수 있게 한다.

```java
//빨간색이면서 150g이상의 사과 또는 녹색인 사과추출
Predicate<Apple> redAndHeavyAppleOrGreen = redApple.and(apple -> apple.getWeight() > 150)
                                                    .or(apple -> Color.GREEN.equals(a.getColor()));
```

이런식으로 활용이 가능하다.

이 외에도 `Function`을 이용한 조합도 있다.

```java
public class Letter {

    public static String addHeader(String text){
        return  "From minseok, java, kotlin" + text;
    }

    public static String addFooter(String text){
        return text + "Kind regards";
    }

    public static String checkSpelling(String text){
        return text.replaceAll("labda", "lambda");
    }
}


@Test
void Function_letter_test(){
    Function<String, String> addHeader = Letter::addHeader;
    Function<String, String> transformationPipeline = addHeader.andThen(Letter::checkSpelling)
            .andThen(Letter::addFooter);
    System.out.println(transformationPipeline.apply("안녕하세요."));
}

//결과: From minseok, java, kotlin안녕하세요.Kind regards
```

이는 모두 `java.util`에 있으며 해당 인터페이스를 까보면서 제공되는 메서드를 확인할 수도 있다.

![image](https://user-images.githubusercontent.com/30401054/230142957-156779af-0fda-440c-a967-cd9a6782f0be.png)

