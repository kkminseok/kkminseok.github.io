---
title: 이것이 리눅스다. - 8. 원격지 시스템 관리하기
author: 강민석
date: 2021-01-14 15:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **8.1 텔넷 서버** ##

- 지금은 인기가 없지만 전통적으로 사용되어 온 원격 접속 방법입니다.
- 리눅스 서버에 텔넷 서버를 설치했다면, 원격지에서 리눅스 서버에 접속할 PC에는 텔넷 클라이언트 프로그램이 필요합니다.
- 운영체제 대부분은 기본적으로 텔넷 클라이언트 프로그램이 내장되어 있으므로 별 문제는 없습니다.

**텔넷 서버 구축**

root
```console
yum -y install telnet-server //텔넷 서버 설치

systemctl start telnet.socket // 서비스 시작
systemctl status telnet.socket // 서비스 상태 확인
```

![](/assets/img/sample/Linux/ThisisLinux/C8/telnet.JPG)  

- 1.유저생성

```console
adduser teluser
passwd teluser //password는 teluser로 설정

yum -y install telnet // 텔넷 클라이언트 설치
telnet 서버IP주소 // 텔넷 클라이언트를 이용해서 접속
```

![](/assets/img/sample/Linux/ThisisLinux/C8/telnet2.JPG)  


- 2.window에서 텔넷 접속하기

먼저 텔넷 클라이언트가 설치되어있는 지 확인합니다.  
다음 경로로 들어가 [텔넷 클라이언트]를 체크해줍니다.  

![](/assets/img/sample/Linux/ThisisLinux/C8/win.JPG)  

```console
telnet 서버IP주소 // 해도 안된다. 서버에 방화벽이 막기 때문이다.
```

- 3.서버의 방/화벽인 보안수준 설정을 변경합니다.

```console
firewall-config //방화벽 설정창 열기
```

다음과 같이 바꿔줍니다.  

![](/assets/img/sample/Linux/ThisisLinux/C8/firewall.JPG) 

이후 [옵션]->[firewalld 다시불러오기]를 선택합니다.

```console
systemctl enable telnet.socket // 재부팅 후에도 텔넷 서버가 계속 가동되도록 함.
```

이제 윈도우에서 접속하면 됩니다.  

![](/assets/img/sample/Linux/ThisisLinux/C8/wintel.JPG) 

root계정으로 접속못하게 CentOs가 막아놨는데 root로 접속하고 싶으면 서버에서 한가지 일을 해줘야합니다.

```console
mv /etc/securetty /etc/securetty.bak  //파일을 이름을 바꿔버림

or
su - 를 통해 접속
```

-----  

## **8.2 OpenSSH 서버** ##

- 텔넷과 용도는 같지만 보안이 더 강화된 서버입니다.
- 텔넷은 서버와 클라이언트 사이에 데이터를 전송할 때 암호화를 하지 않아 해킹의 위험이 있는데, 이를 해결하기 위해 사용하는 것이 OpenSSH서버입니다.

**OpenSSH구축**

- 1.CentOs는 기본적으로 서버가 설치되어 있기에 가동시켜주면된다.

```console
systemctl status sshd //서버 가동중인지 확인
```

- 2.client에서 telnet처럼 명령어로 접속합니다.   
```console
//ssh 사용자이름@호스트이름 or ssh 사용자이름@ip주소
```

![](/assets/img/sample/Linux/ThisisLinux/C8/ssh.JPG) 

> 접속이 안되면 방화벽 설정에서 ssh에 체크해줘야합니다.

- 3.window client에서 접속하기

'putty'라는 SW를 받아서 사용하면 됩니다. ssh기본적인 port번호는 22입니다.

-----  

## **8.3 VNC 서버** ##

- 기존 원격지 서버에서 터미널에서 작업을 하는 것이 아닌 그래픽 환경에서 작업할 수 있게 도와줍니다.
- X 윈도에서 제공하는 명령어를 쓸 수가 없기에 필요성이 있습니다.
- 그래픽 화면을 전송하는 원리로 텔넷과 비교했을 때 많이 느려지는 단점이 있습니다.

서버.root에서 다음 명령어를 쳐줍니다.  
```console
yum -y install tigervnc-server
```

- vnc 서버는 접속할 사용자를 지정하고, 사용자에게 디스플레이 번호를 할당해야합니다. 이 예제에서는 centos 사용자, 디스플레이번호 1번으로 지정했습니다.

```console
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
```

- 위의 명령어는 vnc설정파일을 centos 사용자 설정 파일용으로 복사한 것입니다.

```console
vi /etc/systemd/system/vncserver@:1.service //파일 편집
```

![](/assets/img/sample/Linux/ThisisLinux/C8/centos.JPG)  

그리고 'firewall-config' 명령어를 통해 'vnc-server' 서비스를 허용하고 [옵션]->[Firewalld 다시 불러오기]를 선택합니다.  

```console
su - centos // centos 사용자로 접속
vncserver //vnc서버 실행
//password 설정은 알아서
```

![](/assets/img/sample/Linux/ThisisLinux/C8/vnc.JPG)  

이제 클라이언트에서 접속해봅니다.

centos에서 접속해서 vnc 클라이언트 프로그램을 설치합니다.  
```console
su -c 'yum -y install tigervnc'
vncviwer IP주소:디스플레이번호
```

![](/assets/img/sample/Linux/ThisisLinux/C8/result.JPG)  

**윈도우에서 하기**

VNC클라이언트를 다운받고 똑같이 IP주소:디스플레이번호 하면 됩니다.

-----  


