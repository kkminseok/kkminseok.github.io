---
title: 이것이 리눅스다. - 1.실습환경 구축
author: 강민석
date: 2021-01-04 00:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

![](/assets/img/sample/Linux/ThisisLinux/C1/book.JPG)  

책입니다. 군대에 있을 때 산 책이며, 읽어보자 했지만 군대에서 리눅스 운영체제를 다룰 수 없어서 이제야 읽게 되었습니다.  
당시에는 최신 책이었지만, 지금은 4년이 지난 책으로 빠르게 읽어볼 예정입니다.  
<br>
이 책에서는 VMware 이용하여 실습을 진행합니다. AWS를 이용하고 싶었지만, 원할한 진행을 하려면 VMware를 사용해야할 것 같습니다.  
linux server 2대, window clinet 1대, linux client 1대를 가상환경에서 이용한다고 합니다.  

## **1. 실습환경 구축** ##

- 가상환경의 개념 
  **컴퓨터에 설치된 운영체제 안에 가상의 컴퓨터를 만들고, 그 가상의 컴퓨터 안에 또 다른 운영체제를 설치/운영할 수 있도록 제작된 SW**  

VMware를 설치해야하지만 저는 예전에 사용한 oracle virtual box를 사용하였습니다.

virtual box에서 centos7버전을 받으려면 다음 링크에서 iso파일을 다운받아야합니다.  

<http://isoredirect.centos.org/centos/7/isos/x86_64/>

- Server에 RAM 1GB, harddisk 80GB, Network는 NAT,CPU는 싱글코어
- Server(B)에 RAM 512MB, harddisk 40GB, NAT, 싱글코어
- LinuxClient에 RAM 512MB, hard 40GB, NAT, 싱글코어
- windowClient에 RAM 1GB, hard 20GB, NAT, 싱글코어 , 버전은 8버전이상 으로 맞춰줍니다.

![](/assets/img/sample/Linux/ThisisLinux/C1/vm.JPG)  


centos7버전 설치과정은 다음 블로그를 참고해주세요.
<https://jackerlab.com/virtualbox-centos7-install/>  

그 다음 VMWare에 대한 장점에 대해 설명하고 있습니다.  
스냅숏 기능, 여러 OS를 킬 수 있다는 점, 하드디스크 탈부착으로 인한 장점, PC상태를 그대로 저장해 놓고 재실행 시 그 지점으로 돌아간다는 점 등 있습니다.  

-----

## **2. 가상머신 IP 설정** ##
실습을 요구하는 책에서는 IP를 192.168.111.??로 지정하길 원하기 때문에 ip설정을 해줘야합니다.  
<br>
책과 달리 저는 virtual box를 쓰므로 다른 방법으로 ip설정을 해주었습니다. 
네트워크 관리자를 통해 수동으로 수정해주었습니다.  
<https://www.runit.cloud/2020/02/virtualbox-host-only-network-setting.html>  

네트워크를 설정했는데, centos 설치과정에서 네트워크 ip가 변경되어 일단 이 상태로 진행하기로 했습니다.  

-----

centos 설치 시 오류나는 경우 iso파일이 깨진 가능성이 크므로, 재설치를 하거나 다른 링크로 들어가 설치하셔야합니다.
위의 방법으로 해결이 안될 경우 부팅 디스크에 넣지 않고 controllerIDE에 넣습니다.
![](/assets/img/sample/Linux/ThisisLinux/C1/controller.JPG)


책의 Chapter2는 리눅스 소개인데, 저는 어느정도 아는 내용이므로 스킵하고 바로 다음장으로 넘어가겠습니다.
