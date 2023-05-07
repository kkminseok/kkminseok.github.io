---
title: 2번 읽는 Modern Java In Action - Chapter06 스트림으로 데이터 수집
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-02 00:02:00 +0800
categories: [Java,Modern Java In Action]
tags: [Modern Java In Action]
math: true
mermaid: true
image: 
    path: 
comments: true
---

## 🔅 개요

이전 챕터에서는 `collect`메서드로 `Collector`인터페이스 구현을 전달하였는데, 해당 인터페이스는 스트림의 요소를 어떤 식으로 도출할지를 지정해준다. 

함수형 프로그래밍은 **무엇**을 원하는지 직접 명시할 수 있어서 명령형 프로그래밍과 다르게 코드가 좀 더 간결하고 가독성이 향상된다. 자바에서 제공하는 함수형 API를 살펴볼 챕터인것 같다.

