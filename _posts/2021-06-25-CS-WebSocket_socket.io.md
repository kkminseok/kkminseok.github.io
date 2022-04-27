---
title: CS-WebSocket,Socket.io정리
author: 강민석
date: 2021-06-25 01:34:50 +0800
categories: [기술블로그]
tags: [Network]
math: true
mermaid: true
image: 
comments: true
---
 

2011.12.22 Naver D2글

WebSocket, socket.io에 관하여 - <https://d2.naver.com/helloworld/1336>

을 제 입 맛대로 정리한 글입니다.

-----


## 웹 소켓(Web Socket)이 있기까지

1989년 CERN에서 웹 역사가 시작 되었을 때 사용자와의 상호작용은웬 개발에서 큰 부분을 차지하지 않았다. → 점점 차지하게 되어 나오게 됨.

브라우저 렌더링 방식은 HTTP 요청에 대한 HTTP 응답을 받아서 브라우저 화면을 지우고 다시 그리면서 **깜빡임**이 생기게 된다. 이러한 깜빡임 없이 원하는 부분만 그리며 실시간으로 사용자와 상호작용하는 방식이 나타나고 사용자와 상호작용하는 웹 서비스를 선호하는 사용자가 증가하면서 RIA(Rich Internet Application) 기술의 발달이 촉진.

- RIA?

    위키 정리 : 웹 애플리케이션의 장점은 유지, 웹 브라우저 기반 인터페이스의 단점인 늦은 응답 속도, 데탑 앱에 비해 떨어지는 조작성 등을 개선하기 위한 기술의 통칭 ex) Ajax, 

상호작용하는 웹 서비스를 위해 숨겨진 프레임(Hidden Frame)을 이용한 방법이나 Long Polling, Stream 등 다양한 방법을 사용. 

- Long Polling, Stream ?

    Polling : Polling 방식은 클라이언트가 주기적으로 웹서버에게 새로운 내용이 있는 지 물어보는 방식

    Long Polling : 클라이언트가 웹서버에게 새로운 내용이 있는 지 요청시 **웹서버에 내용이 없다면 대답해주지 않다가 새로운 내용이 생기면 대답해주는 방식**

    Streaming : 클라이언트와 서버가 계속 접속을 유지한 상태에서 이벤트가 발생할 때마다 클라이언트로 데이터를 보내는 방식

    그림 및 참고 : <[https://blog.naver.com/cache798/220895150211](https://blog.naver.com/cache798/220895150211)>

하지만 위의 방식들은 단방향 메시지 교환이기에 상호작용하는 웹 페이지를 복잡하고 어려운 코드로 구현해야 했음.

보다 쉽게 상호작용하기 위한 양방향 메시지 송수신이 필요. 때문에 HTML5 표준안의 일부로 WebSocket API가 등장.

WebSocket : 소켓을 이용하여 자유롭게 데이터를 주고 받을 수 있음.

## WebSocket 프로토콜

WebSocket의 API는 W3C(World Wide Web Consortium)에서 관장, 프로토콜은 IETF(Internet Engineering Task Force)에서 관장

다른 HTTP 요청과 마찬가지로 80번 포트를 통해 연결, 당연한 이야기지만, 클라이언트인 브라우저와 마찬가지로 웹 서버도 WebSocket 기능을 지원해야 한다.

```
GET /... HTTP/1.1  
Upgrade: WebSocket  
Connection: Upgrade
```

브라우저는 Upgrade : WebSocket 헤더 등과 함께 랜덤하게 생성한 키를 서버에 보냄. 서버는 키를 바탕으로 토큰을 생성한 후 브라우저에 돌려줌. 이런 과정으로 WebSocket 핸드 쉐이킹이 이뤄짐.

그 뒤 Protocol Overhead 방식으로 웹 서버와 브라우저가 데이터를 주고 받음. 

- Protocol Overhead ?

    여러 TCP 커넥션을 생성하지 않고 하나의 80번 포트 TCP커넥션을 이용하고, 별도의 헤더 등으로 논리적인 데이터 흐름 단위를 이용하여 여러 개의 커넥션을 맺는 효과.

## Socket.io ?

WebSocket은 다가올 미래의 기술이라 써 볼 수 있는 기술이 아니다.

그에 비해 Socket.io는 바로 사용할 수 있는 기술. JS를 이용하여 브라우저 종류에 상관없이 실시간 웹을 구현할 수 있도록 한 기술.

Socket.io는 WebSocket, FlashSocket, AJAX Long Polling ... 등들을 하나의 API로 추상화한 것. 브라우저와 웹 서버의 종류 버전을 파악하여 가장 적합한 기술을 선택하여 사용. Flash Plugin이 설치 되어 있으면 FlashSocket을 사용하고, 없으면 AJAX Long Polling 방식을 사용.