---
title: Spring - 1 프로젝트 환경설정(1)
author: 강민석
date: 2020-12-24 01:00:00 +0800
categories: [Spring, kyhT&Novice&WebMVC]
tags: [Spring Novice]
math: true
mermaid: true
image: 
comments: true
---

**해당 자료는 인프런 김영한님의 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술 강의 노트입니다.**

## **1. 프로젝트 환경 설정** ##

먼저 JAVA 11을 다운받아줍니다.
저는 밑의 과정대로 진행하였습니다.
<https://zetawiki.com/wiki/%EC%9C%88%EB%8F%84%EC%9A%B0_OpenJDK_11_%EC%84%A4%EC%B9%98>

그다음 해당 강의에서는 IntelliJ를 사용하므로 intlliJ를 다운받아줍니다.<br>
<https://www.jetbrains.com/ko-kr/idea/download/download-thanks.html?platform=windows&code=IIC>

스프링 부트 스타터사이트로 이동해서 스프링 프로젝트를 생성합니다.
<https://start.spring.io/>

설정 부분은 다음과 같습니다.
![](/assets/img/sample/Spring/C1/env.JPG)
모든 설정을 완료한 뒤 하단의 GNERATE를 눌러주시고, 자신이 기억할 수 있는 폴더에 압축을 풀어줍니다.

완료한 뒤, IntelliJ를 켜서 해당 폴더를 열어줍니다.<br>
![](/assets/img/sample/Spring/C1/intell.JPG)  
왼쪽 상단 폴더 트리에서 src -> main -> java -> 프로젝트 -> 프로젝트application을 열어줍니다.
![](/assets/img/sample/Spring/C1/intell2.JPG)
초록색 버튼을 눌러 RUN을 시켜주면
![](/assets/img/sample/Spring/C1/intell3.JPG)
와 같은 창이 뜹니다. ports. 8080 이렇게 뜨는데 아마 로컬컴퓨터 포트 8080에 무슨 일이 일어난 것 같습니다.<br>
해당 코드를 실행한 상태에서
<http://localhost:8080/>를 주소창에 입력해주시면

![](/assets/img/sample/Spring/C1/result.JPG)
<br>
와 같이 뜨면 성공입니다.


++ **빌드 빠르게 하는 법**<br>
영상에 나온 방법입니다.
IntelliJ 메뉴에서 File -> Settings에 들어간다음 Build, Execution, Deployment -> Build Tools -> Gradle에 들어가시면 다음과 같이 뜹니다.

![](/assets/img/sample/Spring/C1/setting.JPG)  
제가 밑줄친 Build and run using 과 Run Test using 을 IntlliJ IDEA로 바꿔줍니다.


