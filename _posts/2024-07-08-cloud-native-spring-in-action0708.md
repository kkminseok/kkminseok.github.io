---
title: 클라우드 네이티브 인 액션(3) - 설정관리
author: minseok
date: 2024-07-08 00:02:00 +0800
categories: [Spring]
tags: [Spring]
math: true
mermaid: true
image: 
    path: /assets/img/cloudNativeSpringInAction/main.jpeg
    alt: Cloud Native Spring In Action
comments: true
---

> 클라우드 네이티브 스프링 인 액션 서적의 데모 프로젝트를 모방하였습니다.
[깃 레포지토리](https://github.com/kkminseok/spring-cloud-native-example)
{: .prompt-info}


저번 포스팅에서는 간단한 컨트롤러, 서비스, 레포지토리를 만들고 테스트 및 테스트코드 작성을 하였다.

이번 챕터에서는 서버 설정관리에 대해서 학습을 하는 것 같다.

실제 운영환경에서는 외부 api의 url이 변경되거나 DB url이 변경되는 등의 상황이 생기면 해당 설정을 바꾸고 재빌드하여 서버를 다시 띄웠을 경험이 있다.

이번 챕터에서는 Spring-Config-Server를 이용하여 런타임때에도 설정정보를 갱신하여 작업할 수 있도록 도와준다.

## □ 속성 사용해보기

Spring의 프로파일을 이용하여 속성을 설정할 수도 있지만 

커맨드라인 또는 코드단에서 직접 속성을 지정



