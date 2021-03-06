---
title: Spring - 2 스프링 핵심 원리 1(1)
author: 강민석
date: 2021-01-18 10:00:00 +0800
categories: [Spring,KYHT&Novice&Point&Basic]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한 선생님의 스프링-핵심-원리-기본편 강의노트입니다.**

-----

## **1. 프로젝트 생성** ##

- 인터페이스와 객체를 나누어서 예제를 만듭니다.

- java로 먼저 만들고 유연하게 작동하는지 먼저 확인합니다.

- <https://start.spring.io/> 이 링크에서 다음과 같이 설정해 준 뒤, Generate를 눌러줍니다.

![](/assets/img/sample/Spring/kyh_Point/C1/setting.JPG)  

- 적당한 폴더에 압축을 풀어준 뒤, Intellij에서 해당 프로젝트의 build.gradle 파일을 불러옵니다.  
- 라이브러리의 설치가 끝난 뒤 실행을 해보아서 잘 작동되는 지 확인합니다.

![](/assets/img/sample/Spring/kyh_Point/C1/result.JPG)  

- 실행시간을 빨리하기 위해 File -> setting에 들어가 'gradle'을 검색한 뒤 'Build and run using', 'Run tests using'을 **Intellij IDEA**로바꿔줍니다.

![](/assets/img/sample/Spring/kyh_Point/C1/gradle.JPG)  

-----  

## **2. 요구사항 설계** ##

- 회원
    + 회원을 가입하고 조회할 수 있다.
    + 회원은 일반과 VIP 두 가지 등급이 있다.
    + 회원 데이터는 자체 DB를 구축할 수 있고, 외부 시스템과 연동할 수 있다. (미확정)
- 주문과 할인 정책
    + 회원은 상품을 주문할 수 있다.
    + 회원 등급에 따라 할인 정책을 적용할 수 있다.
    + 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용해달라. (나중에 변경 될 수 있다.)
    + 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고, 오픈 직전까지 고민을 미루고 싶다. 최악의 경우 할인을 적용하지 않을 수 도 있다. (미확정)

**위의 예제는 김영한 선생님의 스프링-핵심-원리 강의 예제입니다. 출처!!**

- 순수 java로만 작성할 것이며 스프링 부트를 사용한 것은 프로젝트 환경설정을 편히 하기 위함이라고 합니다.!!

-----  

## **3. 회원 도메인 설계** ##

- 강의에서 위의 요구사항을 토대로 회원에 대한 객체 다이어그램, 클래스 다이어그램, 설계에 대한 설명을 해주셨습니다.
**그대로 블로그에 옮기는 것은 아닌 것 같아서 옮기지 않겠습니다.**

-----  

## **4. 회원 도메인 개발** ##

- 'member'라는 package를 만들고 일반 회원과 VIP 등급을 넣기 위한 'Grade' enum을 만듭니다.

![](/assets/img/sample/Spring/kyh_Point/C1/gradle2.JPG)  

```java
package hello.core.member;

public enum Grade {
    BASIC, //일반 회원
    VIP //VIP
}

```

- '회원' 객체를 만들기 위한 'Member' 클래스를 만듭니다.

![](/assets/img/sample/Spring/kyh_Point/C1/Member.JPG)  

- Member 클래스는 id(인덱스), name(이름), grade(회원등급)을 멤버로 갖습니다.
- 윈도우에서는 Alt + Insert 키로 생성자와 get(),set() 함수를 만들어 줍니다.

```java
package hello.core.member;

public class Member {

    private Long id;
    private String name;
    private Grade grade;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Grade getGrade() {
        return grade;
    }

    public void setGrade(Grade grade) {
        this.grade = grade;
    }

    public Member(Long id, String name, Grade grade) {
        this.id = id;
        this.name = name;
        this.grade = grade;
    }

}

```

- 멤버 서비스 객체는 저장소에 의존하므로 저장소를 먼저 만들어줍니다.

- 'MemberRepository' 인터페이스를 만들어줍니다.

![](/assets/img/sample/Spring/kyh_Point/C1/MemberRe.JPG)  

- 인터페이스 구현체 중 하나인 'MemoryMemberRepository' 클래스를 위와같은 방식으로 만들어 줍니다.

```java
package hello.core.member;

import java.util.HashMap;
import java.util.Map;

public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long,Member> store = new HashMap<>();

    @Override
    public void save(Member member) {
        //hash MAP에 넣습니다.
        store.put(member.getId(),member);
    }

    @Override
    public Member findById(Long memberId) {
        //hash MAP에서 꺼냅니다.
        return store.get(memberId);
    }
}

```

- 마지막으로 'MemberService' 인터페이스를 만들어 줍니다.
- **위의 저장소들은 DB가 정해지지 않아 인터페이스를 따로 구축한 것인데 service는 변동사항이 없어 인터페이스를 안만들어줘도 됩니다. 하지만 이상적으로는 모든 구체에 인터페이스를 작성하는 것이 좋다고 합니다.**

```java
package hello.core.member;

public interface MemberService {
    void join(Member member);
    Member findMember(Long memberId);
}

```

- 'MemeberServiceImpl' 클래스를 만들어줍니다.
- 서비스와 같이 구체가 1개인 경우 뒤에 Impl을 적어주는 것이 관례라고 합니다.

```java
package hello.core.member;

public class MemberSerivceImpl implements MemberService{

    //저장소를 가져다 쓸 것이므로
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    @Override
    public void join(Member member) {
        //받아온 member를 save()을 통해 저장합니다.
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        // 받은 ID를 토대로 리턴합니다.
        return memberRepository.findById(memberId);
    }
}
```

-----  




