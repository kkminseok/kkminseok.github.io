---
title: Spring - 2 웹개발 기초
author: 강민석
date: 2020-12-26 00:00:00 +0800
categories: [Spring, kyhT&Novice&WebMVC]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**

## **1. 스프링 부트의 정적 컨텐츠** ##

정적컨텐츠는 파일을 웹브라우저에 그대로 내려주는 형식을 말합니다.  
요즘은 mvc방식을 사용하는데, html을 서버에서 데이터 처리 후 웹브라우저에 내려주는 형식입니다.  
정적컨테츠를 사용하는 방식은 다음과 같습니다.  
![](/assets/img/sample/Spring/C2/static.JPG)  
경로에 들어간 뒤
hello-static.html을 만들어줍니다.

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>static content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```
코드를 작성해줍니다. <http://localhost:8080/hello-static.html>에 들어가시면 확인하실 수 있습니다.  
![](/assets/img/sample/Spring/C2/result.JPG)

스프링에서 동작하는 방식은 먼저 hello-static을 스프링에 넘기고 컨트롤러에서 **먼저** hello-static이란 컨트롤러를 찾습니다.<br> 컨트롤러가 없으면 resources에 있는 hello-static을 찾고 정적컨텐츠를 뿌려줍니다.<br>
위의 코드에서는 hello-static이란 controller가 없기때문에 정적컨텐츠를 뿌려준 방식입니다.

## **2. MVC와 컨트롤 엔진** ##
MVC란 Model, View, Controller입니다.<br>
View란 홈페이지를 그리는데에 집중합니다.<br>
Model과 Controller는 데이터 처리 등 비즈니스에 집중합니다.<br>

간단한 예제를 만들어보겠습니다.

![](/assets/img/sample/Spring/C2/controller.JPG)  
에 들어가 HelloController 파일에 다음과 같이 코드를 쳐줍니다.
```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class HelloController {

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam(value = "name",required = true) String name,Model model){
        model.addAttribute("name",name);
        return "hello-template";
    }
}

```
이제 View를 작성해야합니다.
/resources/templates에 들어가 hello-template.html을 만들어줍니다.<br>
![](/assets/img/sample/Spring/C2/template.JPG)  

코드를 작성해줍니다.
```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

<http://localhost:8080/hello-mvc>로 들어가면 오류가 뜹니다.<br>
왜냐하면 위에서 작성한 Controller에 helloMvc 파라미터를 보시면 'required = true'라고 되어있습니다.<br> 이 뜻은 뒤에 작성한 name에 대한 요청값을 무조건 받아야한다고 명시한 것입니다. 때문에
<http://localhost:8080/hello-mvc?name=spring>이런식으로 작성하셔야합니다.<br> 'spring' 말고도 'hello' 등 이런식으로 바꾸셔도 됩니다.
![](/assets/img/sample/Spring/C2/result2.JPG)  


## **3. API** ##

위에서 작성한 controller에서 다음 코드를 추가햊부니다.

```java
@Controller
public class HelloController {
 @GetMapping("hello-string")
 @ResponseBody
 public String helloString(@RequestParam("name") String name) {
 return "hello " + name;
 }
}
```
이렇게 작성하시면  return으로 본인이 작성한 '내용'인 hello'내용'이 출력됩니다.  
ex)<http://localhost:8080/hello-string?name=sentences>  
@ResponseBody를 사용하면 위에서 호출되는 viewResolver가 호출되지 않으며 HTTP의 BODY에 문자 내용을 직접 반환하는 형식으로 됩니다.  
<br> 
좀 더 직관적인 객체를 넘겨주는 예제를 들겠습니다. 똑같은 controller에 다음 코드를 추가해줍니다.
```java
@Controller
public class HelloController {
 @GetMapping("hello-api")
 @ResponseBody
 public Hello helloApi(@RequestParam("name") String name) {
 Hello hello = new Hello(); //객체 생성
 hello.setName(name); //객체 값 설정
 return hello; //객체를 넘겨줌.
 }
//예제 클래스 작성
 static class Hello {
 private String name;
 //get
 public String getName() {
 return name;
 }
 //set
 public void setName(String name) {
 this.name = name;
 }
 }
}
```
객체를 넘겨줄 땐 JSON형식과 XML형식으로 넘겨주는데, 별다른 지시가 없으면 JSON형태로 넘겨줍니다.  
 JSON은 키와 값을 가진 데이터형식이라 생각하시면 편합니다.  
ex)<http://localhost:8080/hello-api?name=Spring!>  
![](/assets/img/sample/Spring/C2/JSON.JPG)  

동작 방식을 설명하겠습니다.
@ResponseBody를 사용하면 'ViewResolver' 대신에 'httpMessageConverter'가 동작합니다.<br> 문자열을 넘기면 'httpMessageConverter'속에 있는
'StringConverter'가 객체를 넘기면 'JsonConverter'가 호출됩니다.
해당 데이터에 맞게 알아서 호출되는 식입니다.<br> 자세한 내용은 추후 다루겠습니다.



