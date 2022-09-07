---
title: Intellij에서 Spring framework추가하기 (Add Frameworks에 없을 때)
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-09-07 00:02:00 +0800
categories: [Java,Spring_CS]
tags: [Spring]
math: true
mermaid: true
image: 
    path: /assets/img/sample/Spring/CS/spring_logo.png
comments: true
---

![](/assets/img/sample/Spring/CS/Spring_Intellij/init_project.png)


이렇게 그냥 빈 프로젝트를 생성했을때 Spring Frame work을 추가하고 싶을때가 있다.

아니면 아차 싶어서 못했을때 프로젝트를 다시 파는 삽질을 하는 사람도 있을것이다.

보통 프로젝트 상단 우클릭에서 `Add Frameworks Support`를 클릭하여 프레임워크를 찾는 경우가 있다.


![](/assets/img/sample/Spring/CS/Spring_Intellij/addFrameWork.png)

보통 여길 누르면 SpringFramework가 뜨는 데 뜨질 않는다.


![](/assets/img/sample/Spring/CS/Spring_Intellij/whereSpring.png)

이럴경우 따로 등록해줄 수 있는데

프로젝트 상단 우클릭에서 `Open Module Settings` -> `Libraries`에 들어간다.

그리고 `+`버튼을 누른다.

![](/assets/img/sample/Spring/CS/Spring_Intellij/newmaven.png)

여기서 검색할 수 있는 창이 뜨는데, 나는 Spring-mvc를 쓸 것이므로 spring-mvc를 찾는다.

![](/assets/img/sample/Spring/CS/Spring_Intellij/springserach.png)

그리고 다 OK OK OK하면 설정한 라이브러리를 다운받는다.

![](/assets/img/sample/Spring/CS/Spring_Intellij/success.png)

빨간줄도 안 쳐지고 import도 잘 해온다. 성공.

이 방식으로 통해서 매 번 받아오는건 곤란하므로 그냥 `Add Frameworks Support`에서 `maven`을 추가하고 pom.xml을 작성하는것도 방법이다.

