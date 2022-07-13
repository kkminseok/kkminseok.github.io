---
title: Docker - 도커 네트워크
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-07-13 03:00:00 +0800
categories: [Docker,CS]
tags: [Docker]
math: true
mermaid: true
image: 
  path: /assets/img/DockerPost/docker.png
comments : true
---


> 시작하세요! 도커/쿠버네티스 책 정리글입니다.
{: .prompt-tip }

# 📖 도커 네트워크

컨테이너 내부에서 `ifconfig`을 입력하면 eth0과 lo 네트워크를 가지고 있는 것을 확인할 수 있습니다.

도커는 컨테이너에 내부 IP를 순차적으로 할당하고, 이 IP는 컨테이너를 재시작 할 때마다 변경됩니다.

이 IP들은 내부 망에서만 쓸 수 있기에 외부와 연결을 허용할 수 있게끔 바꿔주는 방법이 필요합니다.

각 컨테이너는 시작될 때 도커 엔진이 `veth`라는 이름의 네트워크 인터페이스를 생성하고 이를 통해 외부와 통신할 수 있게 도와줍니다.

도커가 설치된 호스트에서 `ipconfig`이나 `ifconfig`으로 네트워크들을 확인할 수 있습니다.

그러면 `veth`로 시작하는 인터페이스들이 있는데 이것들은 각 컨테이너의 `eth()`와 연결되어있습니다.

veth 인터페이스 뿐만 아니라 `docker0`라는 브리지도 존재하는데 각 `veth` 인터페이스와 바인딩 되어 호스트의 `eth0`인터페이스와 이어주는 역할을 합니다. 

그렇기에 다음과 같은 구조를 가집니다.