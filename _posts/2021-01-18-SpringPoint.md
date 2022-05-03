---
title: Spring - 1 객체 지향 설계와 스프링
author: 강민석
date: 2021-01-18 09:00:00 +0800
categories: [Java,3. Spring_김영한_스프링-핵심-원리-기본편]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **1. 스프링 역사** ##

- EJB라는 것을 초창기에 썼었는데, 그 당시 이론 자체는 좋았으나 비쌌고, 가장 큰 문제는 어렵고 복잡했다고 합니다.
- 두 명의 개발자(Gavin King, Road Johnson)가 EJB를 비판하면서 만든 것이 각각 하이버네이트와 스프링을 만들었습니다.
- 로드 존슨이 책을 집필하면서 3만줄의 스프링 기반코드를 뿌려 개발자들이 그 코드들을 가져다 쓰기 시작했습니다. 유겐 휠러라는 개발자와 얀 카로프는 책으로 냅두기 아까워서 로드 존슨에게 오픈소스 프로젝트를 제안하였고, 유겐 휠러가 스프링의 핵심 코드를 지금도 개발하고 있습니다.
- 스프링의 이름은 J2EE(EJB)라는 겨울을 넘어 새로운 시작이라는 뜻으로 지었다고 합니다.
- 스프링은 처음 XML을 지원하여 XML 편의 기능을 지원했는데, 2009년 스프링 프레임워크 3.0 에서 자바를 사용할 수 있게 만들었습니다. 그리고 2014년 스프링을 편하게 쓸 수 있는 스프링부트가 출시되었습니다.

-----  


## **2. 스프링이란?** ##

- 스프링은 여러가지의 기술이 있습니다. 필수적으로는 프레임워크, 부트가 있고 선택적으로 밑과 같은 기술들이 있습니다.
    + 스프링 데이터, 세션, 시큐리티, Rest Docs, 스프링 배치, 클라우드 등 이 있습니다.
- 스프링 프레임 워크는 다양한 기술이 있습니다.
    + 스프링 DI 컨테이너, MVC, 트랜잭션, 캐시, 테스트 지원, 코틀린 또는 그루비 언어 등이 있습니다.
- 스프링 부트의 특징은 다음과 같습니다.
    + 스프링을 편리하게 사용할 수 있도록 지원합니다.
    + Tomcat 같은 웹 서버를 내장해서 별도의 웹 서버를 설치하지 않아도 됩니다. 
    + starter를 통한 손쉬운 빌드가 가능해집니다.
    + 버전에 맞는 외부 라이브러리를 알아서 가져옵니다.
- 스프링 부트는 스프링 프레임워크를 사용해서 도와주는 기술이고 단독적으로 쓰기에는 어렵다고 합니다.
- 스프링은 스프링 DI 컨테이너 기술, 프레임웤, 그 모두를 포함한 의미를 스프링이라고 이야기할 수 있습니다.

**스프링은 객체 지향 언어인 자바 언어 기반의 프레임워크 입니다. 객체 지향이 가진 특징을 가장 크게 살릴 수 있는 것이 스프링입니다. 이를 토대로 좋은 객체 지향 애플리케이션을 만들 수 있게 됩니다. 이것이 스프링의 핵심입니다.**

-----  

## **3. 좋은 객체 지향 프로그래밍이란?** ##

- 객체는 메시지를 주고 받고 데이터를 처리합니다. 또한 객체 지향 프로그래밍은 프로그램을 유연하고 변경이 용이하게 만들기가 가능해서 개발에 많이 사용합니다.
- 클라이언트는 대상의 역할(인터페이스)만 알면됩니다.
- 클라이언트는 구현 대상의 내부 구조를 몰라도 됩니다.
- 클라이언트는 구현 대상의 내부 구조가 변경되어도 영향을 받지 않습니다.
- 클라이언트는 구현 대상 자체를 변경해도 영향을 받지 않습니다.
- 객체를 설계할 때 역할과 구현을 명확히 분리해야합니다.
- 클라이언트(요청)가 중요합니다. 
- 자바 언어의 다형성은 대표적으로 오버라이딩이 있습니다.
- 스프링은 다형성을 편리하게 사용할 수 있도록 도와주는 것입니다.

-----  

## **4. SOLID이란?** ##

- SRP 단일 책임 원칙
    + Single responsibility principle
    + 계층으로 나누어서 코딩 합니다. 하나의 범위 안에 DB접근 기술, 뷰 설계 등 다 들어가 있는건 안좋은 설계입니다.
    + 변경이 있을 때 하나의 클래스만 고치면 단일 책임 원칙을 잘 따르는 것

- OCP 개방 폐쇄 원칙
    + Open/close principle
    + 확장에는 열려 있으나 변경에는 닫혀있어야 합니다.
    + 다형성을 생각해보면 그렇습니다.
    > 제 생각에는 확장을 할 때 다른 객체까지 건들지 않고 하는 것이고 변경에는 변경 코드가 다른 객체에 영향을 안주는게 좋은 코드라고 말하는 것 같습니다.

- LSP 리스코프 치환원치
    + Liskov substitution principle
    + 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 합니다.
    + 예시로 자동차 엑셀을 밟으면 속도가 증가해야하는데 뒤로가게 구현하면 위의 원칙을 위반하는 행위입니다.

- ISP 인터페이스 분리 원칙
    + Interface segregation principle
    + 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫습니다.
    + 사용자 클라이언트 -> 운전자 클라이언트, 정비사 클라이언트로 분리
    + 인터페이스가 명확해지고, 대체 가능성이 높아집니다.

- DIP 의존관계 역전 원칙
    + Dependency inversion principle
    + 프로그래머는 "추상화에 의존해야지, 구체화에 의존하면 안된다." 라는 원칙을 따라는 방법 중 하나입니다.
    + 역할에 의존한다는 말과 같은 말입니다.
    + 예를 들어서 로미오역할이 어떤 배우여야 된다는 것은 안된다는 것입니다. 즉 대체가 안되면 안된다는 말입니다.

- 정리
    + 객체 지향의 핵심은 다형성입니다.
    + 다형성 만으로는 OCP,DIP를 지킬 수 없습니다.
    + 따라서 무언가 더 필요한데, 스프링에서 지원합니다.

-----  

## **5. 객체지향 설계와 스프링** ##

- 스프링은 다음 기술로 OCP와 DIP를 가능하게 지원합니다.
    + DI : 의존 관계, 의존성 주입
    + DI 컨테이너 제공
- 모든 설계에 역할과 구현을 분리해야합니다.
