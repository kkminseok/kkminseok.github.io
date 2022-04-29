---
title: Spring - 4 회원 관리 예제 (2)웹 MVC개발
author: 강민석
date: 2020-12-30 12:00:00 +0800
categories: [Java,1. Spring_김영한_스프링 입문]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**

-----

## **1. 회원 웹 기능 - 홈 화면 추가** ##

![](/assets/img/sample/Spring/C4/domain.JPG)  
간단한 홈페이지를 작성하겠습니다.  
다음 경로에 새로운 자바파일을 생성해줍니다.  
![](/assets/img/sample/Spring/C4/home.JPG)

```java
package hello.hellospring.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
@Controller
public class HomeController {
    @GetMapping("/")
    public String home() {
    return "home";
 }
}
```

Get방식으로 홈페이지를 뿌려줍니다.
뿌려주기 위해서 resources -> templates에 'home.html'을 만들어 줍니다.

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div>
```
회원 가입을 누르면 /members/new 경로로, 회원 목록을 누르면 /members 경로로 이동합니다.  

-----


## **2. 회원 웹 기능 - 회원 등록 추가** ##

전에 작성한 MemberController.java에 다음 코드들을 추가해줍니다.

```java
@Controller
public class MemberController {
    private final MemberService memberService;
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
 }
    @GetMapping(value = "/members/new")
    public String createForm() {
        return "members/createMemberForm";
 }
}
```
'members/new'로 요청이 들어오면 'members/createMemberForm.html'을 찾아서 뿌려줍니다.
해당 경로인 resources -> templates -> members에 createMemberForm.html 파일을 만들어줍니다.  
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
    <div class="container">
        <form action="/members/new" method="post">
            <div class="form-group">
                <label for="name">이름</label>
                <input type="text" id="name" name="name" placeholder="이름을
입력하세요">
            </div>
            <button type="submit">등록</button>
        </form>
    </div> <!-- /container -->
</body>
</html>
```
홈에서 '회원 가입' 버튼을 누르면 다음과 같이 뜨면 됩니다.
![](/assets/img/sample/Spring/C4/regi.JPG)

여기서 '등록' 버튼을 누르면 입력한 값이 저장소에 저장되고, 다시 홈화면을 띄워줘야합니다. 새로운 컨트롤러를 작성해야합니다.  

컨트롤러 안에 새로운 자바파일인 MemberForm.java를 생성해줍니다.  

```java
package hello.hellospring.controller;

public class MemberForm {
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

다시 MemberController.java로 가서 다음 코드를 추가해줍니다.
```java
@PostMapping("members/new")
    public String create(Member form){
        Member member = new Member();
        member.setName(form.getName());
        //값이 잘 전달되었는 지 확인
        System.out.println("member =" + member.getName());

        memberService.join(member);
        //home 화면으로 돌림
        return "redirect:/";

    }
```

-----

## **3. 회원 웹 기능 - 회원 조회 추가** ##

MemberController.java에 다음 조회를 위한 다음 코드를 추가해줍니다.

```java
@GetMapping("/members")
    public String list(Model model){
        List<Member> members = memberService.findMembers();
        model.addAttribute("members",members);
        return "members/memberList";
    }
```

List로 찾은 값들을 모델에 넣어주고 members/memberList.html을 반환합니다.


뿌려주기위해 resources/templates/members에 memberList.html파일을 만들어줍니다.

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <table>
            <thead>
            <tr>
                <th>#</th>
                <th>이름</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each="member : ${members}">
                <td th:text="${member.id}"></td>
                <td th:text="${member.name}"></td>
            </tr>
            </tbody>
        </table>
    </div>
</div> <!-- /container -->
</body>
</html>
```
Thymleaf 문법인 th:each 문으로 리스트를 돌면서 값들을 꺼내옵니다.  
제가 만약 '안녕', '넹', ' ' 등을 넣었다면 F12로 소스를 보면 다음과 같이 나옵니다.

![](/assets/img/sample/Spring/C4/result.JPG)

위의 사진을 보시면, Thymleaf 분법인 th:each가 html문법으로 치환되어 나타나는 것을 볼 수 있습니다.

