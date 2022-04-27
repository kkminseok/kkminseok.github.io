---
title: Spring DAO란?, DTO란?, VO란?, DTO vs VO, DTO vs Domain, DAO vs Repository
author: 강민석
date: 2021-10-06 00:34:50 +0800
categories: [Java,Spring_CS]
tags: [Spring]
math: true
mermaid: true
image: 
comments: true
---

귀염둥이 후배들과 스터디를 하면서 세미나를 하기로 했다.

나는 DAO, DTO, VO, @Entity의 개념을 조사하기로 했다.

---

## DAO(Database Access Object)

즉, 데이터베이스에 접근하는 **객체**이다.

단일 데이터의 접근 및 갱신의 개념.

프로젝트의 구성중 Repositry와 DAO가 비슷하다고 생각하여 조사하였다.(실제로 같다고 보는 사람들도 있다.)

```java
public interface QuestionRepository extends CrudRepository<Question, Long> {
}
```

- Repository vs DAO

    일단 이게 논란이 많은 부분이라고하는 듯.  
    그래서 찾아보면 다 개인적인 견해라고 밝힘.  
    제일 심오하게 고민한 글 : [https://bperhaps.tistory.com/entry/Repository와-Dao의-차이점](https://bperhaps.tistory.com/entry/Repository%EC%99%80-Dao%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)

    각자 읽는 것을 추천. 저 내용을 정리하기엔 제대로 전달할 자신이 없다.

    참고 : <[https://bbbicb.tistory.com/44](https://bbbicb.tistory.com/44)>

### 1. None

- DAO는 data persistence의 추상화
- Repository는 a collection of objects의 추상화.

     먼말인지 모르겠음.. 찾아봐도 잘 안 나온다. data persistence관련은 IBM 홈페이지에 정리되어있음.<[https://www.ibm.com/docs/en/cloud-paks/cp-biz-automation/20.0.x?topic=designer-data-persistence](https://www.ibm.com/docs/en/cloud-paks/cp-biz-automation/20.0.x?topic=designer-data-persistence)>

- DAO는 데이터베이스와 관련이 많다. Table중심.
- Repository는 도메인과 관련이 많다. Arggregate Roots만을 다룬다.  
- DDD와 Aggregate Root란?

### DDD(Domain Driven Design) - 도메인 중심으로 설계하는 디자인 방법론 도메인 주도 설계

- Domain ?
  - 회원가입, 회원탈퇴 등에서 작업은 모두 '회원'의 중심으로 진행됨. 여기서 회원이 도메인. 어플리케이션 내 로직들이 관여하는 정보와 활동의 영역.

  - 옷 쇼핑몰에서 손님들이 주문하는 도메인이 존재할 수도 있고, 점주입장에서는 옷들을 관리하는 도메인이 존재할 수 있다. 즉, 사건이 발생하는 집합들. 추상화된 것들도 대상이 될 수 있다는 것. '주문'이라는 것처럼.

  - 또한 이처럼 문맥에 따라 객체의 역할이 바뀌는 것을 **Bounded Context**라고 부름.

  - Bounded Context에 따라서 Model의 역할은 완벽히 달라지고, 책임 또한 달라짐.

  - DDD가 필요한 이유
    - 데이터에 종속적으로 개발하거나 모델링과 개발이 불일치한 경우를 방지하기 위해 DDD가 등장.

  - 설계 방식?
    - 크게 3가지 Layer로 나눔.
    !["Layers"](/assets/img/sample/JAVA/study/semina/Layer.png)

    - 그 전에 사진에 MicroService가 나왔는데 why?

    - 위에서 Bounded Context에 대해 말했는데, 이러한 관점을 더나아가서 서비스에 적용시킨 것이 **MicroService**라고 한다.

    - 서로 다른 도메인 영역에 영향을 끼치기 위해서는 API 호출로 해야 된다는 것.

    - 각각의 도메인은 서로 철저히 분리되고, **높은 응집력**과 **낮은 결합도**로 변경과 확장에 용이한 설계를 얻게됨. (응집력, 결합도 모르는 사람?)

    - Application Layer : 주로 도메인과 Repository 바탕으로 실제 서비스(API)를 제공하는 계층

    - Domain Layer : Entity, VO를 활용하여 도메인 로직이 진행되는 계층

    - Infrastructure Layer : 쉽게 말해서 외부와의 통신(ORM, DB, NoSQL)을 담당하는 계층

    - DDD의 핵심은 **도메인을 서비스로 별로 분리하라**

    - 하지만 모든 도메인마다 많은 객체를 다루고 각 객체마다 Repository를 만들면 DDD가 가지는 장점을 내세울 수 없게됨. 여기서 등장하는 것이 **Aggregate(집합)**

---

### Aggregate(집합)

- 각각의 도메인 영역을 대표하는 객체를 Aggregate라고 함.

- 그렇게 하면, 각각의 **도메인에 Repository로 묶어야하는 객체(Entity)가 명확해지기 때문.**

- Top - Down 방식으로 계층을 타고타고 내려갈 때 어떤 Entity를 만들기 위해 도메인이 이루어져 있는지 명확하게 들어오기도 함.

- 예
!["aggregate"](/assets/img/sample/JAVA/study/semina/Aggregate.JPG)  
출처 : <[https://huisam.tistory.com/entry/DDD](https://huisam.tistory.com/entry/DDD)>

위의 예시에서 Order라는 Domain에서 Order라는 Entity를 만들기 위해 다양한 객체들이 상호협력하고 있는 관계를 볼 수 있다.  

- 이 Aggreagte 포함되어 있는 특정 Entity를 Aggregate Root라고 한다.  

- 위의 경우에서 루트는(Order)는 하나만 존재하고, Aggregate 내의 특정 엔티티를 가리킨다. 경계 안의 객체는 서로 참조할 수 있지만, 경계 바깥의 객체는 해당 Aggregate의 구성요소 중 루트에만 참조할 수 있다.  

- 위의 방식으로 사용해야 하는 이유는 **데이터 무결성** 측면이 있다. Order가 아닌, shpping address 등에만 데이터를 저장하면 데이터 무결성이 깨질 위험이 있다.

---

## DTO(Data Transfer Object)

DataBase에서 Data를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체.

## VO(Value Object)

각 계층간 데이터 교환을 위한 자바 객체를 의미.

레이어 간에 전달하는 목적을 가지고 있으며, 객체의 속성과 getter, setter만을 가지고 있음.

계층간 데이터 교환을 위한 자바 빈즈(Java Beans)이다.

- 자바 빈즈?
  - 자바로 작성된 소프트웨어 컴포넌트들을 지칭하는 단어
  - MVC모델 기준 Model이 JavaBeans
  - JSP에서도 쓰이는데 JSP문법은 모르니 pass → JSP 파일 내에서 사용이 가능한 객체라고 생각.

즉 DTO는 Database에서 Data를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체.  

- VO(Value Object) vs DTO

VO는 DTO와 동일한 개념이지만 read only 속성을 갖음.  

VO는 특정한 비즈니스 값을 담는 객체이고, DTO는Layer간의 통신 용도로 오고가는 객체를 말함.

---

## Entity - 고유 식별자를 바탕으로 객체의 정체성을 부여

실제 DB의 테이블과 매칭될 클래스

- 즉, 테이블과 링크될 클래스임을 의미.
- Entity클래스 도는 가장 Core한 클래스라 부름
- 최대한 외부에서 Entity 클래스의 getter method를 사용하지 않도록 해당 클래스 안에서 필요한 로직 method를 구현한다.

- Entity와 VO의 차이점
  - Entity는 고유 식별자(Primary key)를 바탕으로 객체의 정체성을 부여.

  - VO는 상태(Attribute)를 바탕으로 객체의 정체성을 부여.
  - equals Hashcode를 **id로만 하면 Entity, 상태에 대한 모든 정보로 하면 VO**

---

## DTO와 Entity를 분리하는 이유

- View Layer와 DB Layer의 역할을 철저하게 분리하기 위해서이다.
- 테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 View와 통신하는 DTO클래스는 자주 변경되므로 분리해야 한다.
- Domain Model을 아무리 잘 설계했다고 해도 View 내에서 Domain Model의 getter만을 이용해서 원하는 정보를 표시하기 어려운 경우가 있다고 함. 이런 경우 Presentation을 위한 필드나 로직을 추가하게 되는데, 이러한 방식이 모델링의 순수성을 깨고 Domain Model 객체를 망가뜨리게 됨.
- 또한 Domain Model을 복잡하게 조합한 형태의 Presentation 요구사항들이 있기 때문에 Domain Modl을 직접 사용하는 것은 어려움.
- 즉 DTO는 Domain Model을 복사한 형태로, 다양한 Presentation Logic을 추가한 정도로 사용하며 Domain Model 객체는 Persistent만을 위해서 사용한다.
- 여기서 Persentation은 View계층, persistent는 인프라스트럭쳐 = DB, 외부 라이브러리 등의 인프라

!["DTODOMAIN"](/assets/img/sample/JAVA/study/semina/DTODOMAIN.JPG)

---

## Controller(web)

- 기능
  - 해당 요청 url에 따라 적절한 view와 mapping 처리
  - @Autowired service 를 통해 Service의 method를 이용
  - 적절한 RsponseEntity(DTO)를 body에 담아 Client에 반환  

- @Controller vs @RestController
  - Controller
    - API와 view를 동시에 사용하는 경우에 사용
    - 대신 API 서비스로 사용하는 경우는 @ResponseBody를 사용하여 객체를 반환
    - View(화면) return이 주목적
  - RestController
    - view가 필요없는 API만 지원하는 서비스에서 사용.
    - @ResquestMapping 메서드가 기본적으로 @ResponseBody 의미를 가정
    - data(json, xml 등) return이 주목적 : return ResponseEntity
    - 즉, @Controller + @ResponseBody

---

## Service

- 기능
  - Autowired를 사용하든 뭘하든 어떠한 방식을 통해 repository의 method를 이용
  - DAO로 DB에 접근하고 DTO로 데이터를 받은 다음, 비즈니스 로직을 처리해 적절한 데이터로 반환

    ```java
    @Service
    public class UserService {
      @Autowired
      private UserRepository userRepository; // -> DAO
      @Resource(name = "bCryptPasswordEncoder")
      private PasswordEncoder bCryptPasswordEncoder; // 비즈니스 로직을 처리하기 위한 변수?
      @Autowired
      private MessageSourceAccessor msa;
    
    //비즈니스로직, userRepo에 접근하고, 예외가 발생할 경우 메시지를 던지는 비즈니스 로직
      public User save(UserDto userDto) {
          if (isExistUser(userDto.getEmail())) {
              throw new UserDuplicatedException(msa.getMessage("email.duplicate.message"));
          }
          return userRepository.save(userDto.toEntityWithPasswordEncode(bCryptPasswordEncoder);
      }
    }
    
    ```
