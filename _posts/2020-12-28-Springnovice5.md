---
title: Spring - 3 회원 관리 예제 (2)
author: 강민석
date: 2020-12-28 01:00:00 +0800
categories: [Spring, kyhT&Novice&WebMVC]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**


**전 포스팅을 참고하시길 바랍니다.**

-----

## **1. 회원 서비스 만들기** ##
![](/assets/img/sample/Spring/C3/service.JPG)  
service package를 만든 후 MemberService.java파일을 만들어줍니다.
```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepositroy;
import hello.hellospring.repository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;

public class MemberService {
    private final MemberRepositroy memberRepositroy = new MemoryMemberRepository();

    /*
    *회원가입 1. 중복 회원 고려
     */
    public Long join(Member member){
        validateDuplicateMember(member);//중복회원 검증 Method refactoring 했음. 
        //밑의 함수를 간단하게 위의 메소드로 작성한 것입니다.
        /*
        Optional<Member> result = memberRepositroy.findByName(member.getName());
        result.ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });
         */
        memberRepositroy.save(member);


        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepositroy.findByName(member.getName())
                .ifPresent(m ->{
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }
    
    /*
    * 전체 회원 조회
     */
    public List<Member> findMembers(){
        return memberRepositroy.findAll();
    }

    public Optional<Member> findOne(Long memberId){
        return memberRepositroy.findById(memberId);
    }

}
```

-----


## **2. 회원 서비스 테스트** ##

먼저 위의 코드에서 수정할 부분이 있습니다.  
이유는 전 포스트에서 했던 방식으로 테스트부분에서 객체를 새로 생성해서 쓰는 방법은 테스트의 목적을 충족시키기 애매하기 때문입니다.  
MemberRepository의 메소드에 접근해야하는데 새로운 객체를 생성해서 메소드를 사용하기 보다는 객체를 이어받아서 하는게 더 깔끔해보입니다.  

-----

MemberRepository의 clearstore() Method를 사용한다고 가정하면
1. 기존 방법 
- MemberRepository 객체생성
- MemberService 객체생성  
- MemberRepository 객체의 clearstore() 메소드 호출.
- 저장소를 비웠으니 MemberService 함수들을 호출.  
**이 방식은 저장소와 service가 독립적으로 작동한다는 것입니다.**

2. 해야할 방법
- MemberRepository 객체생성
- MemberService 객체를 생성할 때 위에서 생성한 MemberRepository 객체를 이어준다.  
**저장소와 service가 이어져 있게 된다.**

-----

위의 방식으로 해야하기 때문에 객체를 Service에서 생성해줘야합니다. 

```java
private final MemberRepositroy memberRepositroy = new MemoryMemberRepository();
```

코드를

```java
//기존의 객체를 사용하기 위해 해당 방법을 쓴다.
    private final MemberRepositroy memberRepositroy;
    public MemberService(MemberRepositroy memberRepositroy){
        this.memberRepositroy = memberRepositroy;
    }
```
로 바꿔줍니다.

그리고   
![](/assets/img/sample/Spring/C3/public.JPG)  
처럼 드래그를 해줍니다. window 환경 기준 ctrl + shift + t를 누르게 되면 자동으로 Test 파일이 생깁니다.

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemoryMemberRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.AssertionsForClassTypes.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

class MemberServiceTest {

    //테스트는 과감하게 한글로 바꿔도 된다. 빌드될 때 포함되지 않음.
    MemberService memberService;
    MemoryMemberRepository memberRepository;

    //실행하기 전 해줘야할 것들 명세
    @BeforeEach
    public void beforeEach(){
        //객체들을 생성하고 이어주는 과정
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }
    //밑의 방식은 객체가 유지되지 않는 다른 방법
    //MemoryMemberRepository memberRepository = new MemoryMemberRepository();
    @AfterEach
    public void afterEach(){
        //객체를 지워준다. 안지워주면 테스트순서에따라 객체를 잘못 조회하여 오류가 난다.
        memberRepository.clearStore();
    }




    @Test
    void 회원가입() {
        //given 무언가 주어졌을 때
        Member member = new Member();
        member.setName("spring");
        //when 어떤 상황에서
        Long saveId = memberService.join(member);
        //then 무언가 나와야한다.
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외(){
        //given
        //중복을 확인하기 위해 Name에 같은 값을 넣어준다.
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");
        //when
        memberService.join(member1);

        //오류를 검출하기 위한 방법. 변수 e에 메시지를 넣어주고 같은지 학인하는 과정
        IllegalStateException e = assertThrows(IllegalStateException.class,()-> memberService.join(member2));

        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");

        /*방법 2 이 방법은 상대적으로 직관적이지 않다.
        memberService.join(member1);
        try {
            memberService.join(member2);
            fail();
        }catch (IllegalStateException e){
            org.assertj.core.api.Assertions.assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
        }
         */
        //then

    }


    @Test
    void findMembers() {
    }

    @Test
    void findOne() {
    }
}
```

이렇게 해주고 마지막으로 테스트를 돌립니다.  
![](/assets/img/sample/Spring/C3/result.JPG)  
가 뜨면 성공.