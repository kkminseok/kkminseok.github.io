---
title: 2번 읽는 Modern Java In Action - Chapter02 동적 파라미터화 코드 전달하기
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2023-03-27 00:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
comments: true
---

동적 파라미터화란 말 그대로 메소드의 파라미터를 동적으로 정해줘서 넘겨주는 것을 말한다.

그래서 자주 바뀌는 요구사항에 효과적으로 대응이 가능하다고 한다. 

## 🔅 예제

책에서는 사과 파는것에 대한 예제를 들고 있다.

만약 빨간사과, 녹색 사과가 있고 녹색사과만 필터링하고 싶으면 어떻게 할까?

```java
@Test
void 녹색사과만필터(){
    List<Apple> inventory = new ArrayList<Apple>();
    inventory.add(new Apple(Color.GREEN));
    inventory.add(new Apple(Color.RED));
    inventory.add(new Apple(Color.GREEN));
    inventory.add(new Apple(Color.RED));

    List<Apple> greenApples = filterGreenApples(inventory);
    assertEquals(greenApples.size(), 2);
}

private List<Apple> filterGreenApples(List<Apple> inventory){
    List<Apple> result = new ArrayList<>();
    for(Apple apple: inventory){
        if(Color.GREEN.equals(apple.getColor())){
            result.add(apple);
        }
    }
    return result;
}
```

이런식으로 코드를 작성할 수 있다. 만약에 요구사항이 바뀌어서 `GREEN`이 아니고 빨간(`RED`)를 필터하려면 어떻게 할까?

`if`문만 바꾸고 나머지 로직은 동일한 `filterRedApples()`메서드를 추가할 수 있을 것이다. 하지만 점점 요구사항이 바뀌면 바뀔수록 이는 반복되는 코드가 너무 많아질 것이다.

아니면 비교할 색을 받는 파라미터를 하나 늘려서 유연하게 대처할 수 있을 것이다.

```java
@Test
void 좀더유연하게필터(){
    List<Apple> inventory = new ArrayList<Apple>();
    inventory.add(new Apple(Color.GREEN));
    inventory.add(new Apple(Color.RED));
    inventory.add(new Apple(Color.GREEN));
    inventory.add(new Apple(Color.RED));
    //파라미터 하나 늘었음.
    List<Apple> greenApples = filterApplesByColor(inventory, Color.RED);
    assertEquals(greenApples.size(), 2);
}

//파라미터 하나 늘었음.
private List<Apple> filterApplesByColor(List<Apple> inventory, Color color){
    List<Apple> result = new ArrayList<>();
    for(Apple apple: inventory){
        if(color.equals(apple.getColor())){
            result.add(apple);
        }
    }
    return result;
}
```

좋은 코드처럼 보인다. 하지만 **색 말고 무게가 150그램 이상인 사과를 거르고 싶다**라는 조건이 생긴다면 어떻게 할까?

비슷하게 코드를 작성하는 방법이 있다.

```java
@Test
void 무게필터(){
    List<Apple> inventory = new ArrayList<Apple>();
    inventory.add(new Apple(130));
    inventory.add(new Apple(170));
    inventory.add(new Apple(160));
    inventory.add(new Apple(150));

    List<Apple> greenApples = filterApplesByWeight(inventory, 150);
    assertEquals(greenApples.size(), 2);
}

private List<Apple> filterApplesByWeight(List<Apple> inventory, int weight){
    List<Apple> result = new ArrayList<>();
    for(Apple apple: inventory){
        if(apple.getWeight() > weight){
            result.add(apple);
        }
    }
    return result;
}
```

하지만 결국 색 필터 관련 함수와 대부분 중복이 된다는 단점이 있다.

책에서는 `flag`인자로 하나 더 둬서 `true`인 경우는 색 필터를 진행하고 `false`인 경우 무게 필터를 적용하는 코드를 작성했는데, 별로 좋은 코드가 아니므로 시간이 아까워 작성하지 않겠다.

**결국 이러한 메서드를 늘려나가는 방식은 한계가 있다는 점을 알려주고 있다.**

## 🔅 동작 파라미터화

선택 조건을 결정하는 인터페이스를 하나 둬보자.

보통 참 또는 거짓을 반환하는 함수를 `predicate`라고 한다.

```java
public interface ApplePredicate {
    boolean test(Apple apple);
}
```

이 인터페이스를 구현한 색 필터 클래스, 무게 필터 클래스를 작성할 수 있다.

```java
public class AppleGreenColorPredicate implements ApplePredicate{
    @Override
    public boolean test(Apple apple) {
        return Color.GREEN.equals(apple.getColor());
    }
}

public class AppleHeavyWeightPredicate implements ApplePredicate{
    @Override
    public boolean test(Apple apple) {
        return apple.getWeight() > 150;
    }
}
```

테스트코드는 다음처럼 작성할 수 있다.

```java
 @Test
void 동작파라미터화(){
    List<Apple> inventory = new ArrayList<Apple>();
    inventory.add(new Apple(130, Color.GREEN));
    inventory.add(new Apple(170, Color.RED));
    inventory.add(new Apple(160, Color.RED));
    inventory.add(new Apple(150, Color.RED));

    List<Apple> greenApples = filterApples(inventory, new AppleGreenColorPredicate());
    assertEquals(greenApples.size(),1);
}

private List<Apple> filterApples(List<Apple> inventory, ApplePredicate p){
    List<Apple> result = new ArrayList<>();
    for(Apple apple: inventory){
        if(p.test(apple)){
            result.add(apple);
        }
    }
    return result;
}
```

분류하는 함수의 파라미터를 보면 `ApplePredicate` 객체 형태의 인자를 받는다. 그리고 실제 호출할때는 우리가 원하는 필터 클래스, 즉 상속받은 클래스를 넣어서 다양한 기능을 수행할 수 있게 한다. 이를 **동작 파라미터화**라고 한다.

이를 통해 반복되는 로직과 각 요소에 적용할 동작, 즉 색이나 무게에 따라 필터를 적용하는것의 코드를 분리할 수 있다.

즉, 적절한 객체를 전달하기만하면 `filterApples`내부에서 이를 확인하여 동작을 결정한다는 것이다.

**물론 여기에도 불편한 점이 있다. 두 개를 섞어서 쓰고 싶거나, 결국 새로운 클래스를 매 번 생성하고 상속받아야한다는 점이다.**

그래도 코드를 분리하고 필요에 따라서 원하는 객체를 인자로 넘겨 필터를 진행할 수 있다는건 알아둬야할 점이라고 생각한다.

## 🔅 익명 클래스의 사용

위에서 말한 불편한 점을 해결하기 위해서 **익명 클래스**를 사용할 수 있다.

익명클래스의 장점으로는 선언과 동시에 인스턴스화를 할 수 있기에 따로 클래스파일을 만들 필요가 없다.

만약, **빨간색**사과만 필터하고 싶다면

```java
@Test
void 익명클래스의_사용(){
    List<Apple> inventory = new ArrayList<Apple>();
    inventory.add(new Apple(130, Color.GREEN));
    inventory.add(new Apple(170, Color.RED));
    inventory.add(new Apple(160, Color.RED));
    inventory.add(new Apple(150, Color.RED));

    List<Apple> redApples = filterApples(inventory, new ApplePredicate() {
        @Override
        public boolean test(Apple apple) {
            return Color.RED.equals(apple.getColor());
        }
    });
    assertEquals(redApples.size(), 3);
}
```

이런식으로 익명 클래스를 통해 작성할 수 있다.

하지만 익명클래스도 결국 코드 수가 길어지게 하는 원인이 되고, 프로그래머들에게 익숙하지 않다고 한다.

책에서는 많은 프로그래머들이 곤경에 빠지는 고전 자바문제를 출제하였는데, 코드는 다음과 같다.

```java
class MeaningOfThis{
    public final int value = 4;
    public void doIt(){
        int value = 6;
        Runnable r = new Runnable() {
            public final int value = 5;
            @Override
            public void run() {
                int value = 10;
                System.out.println(this.value);
            }
        };
        r.run();
    }
}

@Test
void 고전자바문제(){
    MeaningOfThis m = new MeaningOfThis();
    m.doIt();
}
```

이것의 답은 5이다. `run()`에서 말하는 `this`는 `Runnable`을 참조하기 때문이다.

참고로, `this`를 안 쓰면 지역변수가 출력된다.

```java
class MeaningOfThis{
    public final int value = 4;
    public void doIt(){
        int value = 6;
        //4
        System.out.println(this.value);
        //6
        System.out.println(value);

        Runnable r = new Runnable() {
            public final int value = 5;
            @Override
            public void run() {
                int value = 10;
                //5
                System.out.println(this.value);
                //10
                System.out.println(value);
            }
        };
        r.run();
    }
}
```

즉, 이는 코드의 장황함을 가지는 코드이다. 장황한 코드는 알아보기 어렵고 유지보수하는데도 시간이 오래걸리기에 개발자들의 사랑을 받지 못한다. 그러므로 익명 클래스도 만족스러운 방법은 아니다. 결국 과정은 생략하였지만 결국 객체를 생성하고 동작을 정의한다는 점은 바뀌지 않기 때문이다.

결국 **람다**를 이용해서 간단하게 적용할 수 있는데, 

```java
@Test
void 람다맛보기예제(){
    List<Apple> inventory = new ArrayList<Apple>();
    inventory.add(new Apple(130, Color.GREEN));
    inventory.add(new Apple(170, Color.RED));
    inventory.add(new Apple(160, Color.RED));
    inventory.add(new Apple(150, Color.RED));

    List<Apple> redApples = filterApples(inventory, (Apple apple) -> Color.RED.equals(apple.getColor()));
    assertEquals(redApples.size(),3);
}
```

책을 읽고, 다시 복습하며 코드를 짜지만 정말 간단하다고 느낀다.

3장은 람다에 대한 얘기를 한다.

