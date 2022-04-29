---
title: 이것이 리눅스다. - 3.Centos 설치
author: 강민석
date: 2021-01-07 00:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

Chapter2는 거의 아는 내용이라 스킵하였습니다.  
Chapter3는 Centos를 설치하는 것인데, 기존에 사용하던 oracle vritual box가
네트워크 오류가 생겼고, 해결하기 어려워 vmware 16 버전으로 환경설정을 다시하고 있습니다.  

vmware16 무료버전은 네트워크 설정하는 방법들이 편법이므로 그냥 vmware에서 제공하는 ip로 실습을 진행하였습니다.  

Chapter3에서 말하는 환경들은 다음과 같습니다.

## Server
환경을 위해 Centos 업데이트를 끔, network설정, SELinux 기능 끄기
**network 설정 부분에서 버전차이인지 오류가 너무 많이 나서 그냥 진행하기로 했습니다. 구글링을 해본 결과 많은 사람들이 네트워크 관련 부분에서 오류를 많이 겪더군요.**  
flashplayer를 다운받아야하는데, 2020년 이후로 서비스가 종료되었으므로 그냥 진행.. 찾아보니 없어도 된다해서 진행하였습니다.  

## Client


root로 로그인 못하게 막기  
![](/assets/img/sample/Linux/ThisisLinux/C3/client.JPG)  
뭘 잘못했는 지 몰라도 root가 아니여도 접속이 안된다.
결국 client 재설치 했다. root권한 그냥 안막았습니다.

+ flash 깔기 -> 생략했습니다.
+ 서버와 같이 버전 유지하기
+ 자동로그인 기능 활성화

## Server(B)

+ vi로 Server에 해줬던 설정해주기
+ 네트워크 설정(고정 ip 할당) -> 또 오류날 것 같아서 생략
+ 보안 기능 끄기
+ 해상도 변경 -> 생략
+ wget 설치
+ wget을 이용해서 필수적인 패키지 설치

## WinClient

+ window를 설치하기 위해 윈도우 10 평가판.iso을 다운받음
+ window를 좀 더 부드럽게 해준다는 VMware Tools 설치.



-----  

2017년 책이라 그런지 지금과는 다른 부분도 있고 그 부분에서 오류가 많이 발생하는 것 같다.  
뭘하든 환경설정이 항상 오래걸리는 것 같다.  
텐서플로 깔 때도 몇시간 걸린것 같은데..








