---
title: 이것이 리눅스다. - 4. 리눅스 명령어, 개념(3)
author: 강민석
date: 2021-01-11 15:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **4.4 네트워크 관련 설정과 명령어**

- 네트워크 설정 정보를 출력  
```console
ifconfig ens33(장치이름)
```

- 네트워크 장치 가동, 정지  
```console
ifup ens33
ifdown ens33
```

- nmtui(Network Manager Text User Interface)  
    + 자동 IP주소 또는 고정 IP주소 사용 결정
    + IP 주소, 서브넷 마스크, 게이트웨이 정보 입력
    + DNS 정보입력
    + 네트워크 카드 드라이버 설정
    + 네트워크 장치의 설정

- systemctl start/stop/restart/status network  
네트워크 설정을 변경한 후에 변경된 내용을 시스템에 적용시키는 명령어들

- 네트워크 설정과 관련된 주요파일
    + /etc/sysconfig/network -> 네트워크 기본 정보가 설정되어 있는 파일로 네트워크 사용 여부가 써 있다.
    + /etc/sysconfig/network-scripts/ifcfg-ens33 -> 해당 장치에 설정된 네트워크 정보가 들어있다.
    + /etc/resolv.conf -> DNS 서버의 정보와 호스트 이름이 들어 있는 파일
    + /etc/hosts -> 현 컴퓨터의 호스트 이름과 FQDN이 들어 있는 파일

- SELinux
    + 시스템에서 보안에 영향을 미치는 서비스, 권한 등을 제어
    + 강제(enforcing), 허용(permissive), 비활성(disabled) 라는 3가지 레벨을 지원.
    + 설정 파일은 /etc/sysconfig/selinux 파일이다.

-----  

## **4.5 파이프, 필터, 리다이렉션**

- 파이프
    + 파이프는 2개의 프로그램을 연결해주는 통로
    ```console
    ls -l /etc | more -> ls -l /etc + more명령어
    ```
- 필터
    + 필터는 필요한 거만 걸러주는 명령어
    + 'grep', 'tail', 'wc', 'sort', 'awk', 'sed' 등의 명령어가 있다.
    ```console
    ps -ef | grep bash -> bash라는 글자가 들어간 프로세스만 출력하게 된다.
    ```
- 리다이렉션
    + 리다이렉션은 표준 입출력의 방향을 바꿔준다.

-----  

## **4.6 프로세스, 데몬, 서비스**

#### **4.6.1 프로세스**

- 포그라운드 프로세스
- 백그라운드 프로세스
- 프로세스번호
- 작업 번호
- 부모 프로세스, 자식 프로세스
- ps
- kill
- pstree

이름을 들어봤으므로 설명은 적지 않겠습니다.  

#### **4.6.2 서비스**

- 서비스=데몬=서버 프로세스

-----  

## **4.7 서비스와 소켓**

서비스는 평상시에도 늘 가동하는 서버 프로세스, 소켓은 필요할 때만 작동하는 서버 프로세스이다.  
'systemd'라고 부르는 서비스 매니저 프로그램으로 작동시키거나 관리.  

#### **4.7.1 서비스**

- 웹 서버(httpd), DB 서버(mysqld), FTP서버(vsftpd) 등이 있다.
- 서비스의 실행 스크립트 파일은 /usr/lib/systemd/system/ 디렉터리에 '서비스이름.service'로 존재

#### **4.7.2 소켓**

- 서비스와 달리 요청이 끝나면 소켓도 종료됨.
- systemd가 서비스를 새로 구동하는 데 시간이 소요되므로 서비스보다 연결시간이 오래 걸릴 수 있다.
- 소켓의 실행 스크립트 파일은 /usr/lib/systemd/system/ 디렉터리에 '소켓이름.socket'로 존재

-----  


## **4.8 응급복구**

root 사용자의 비밀번호를 잊어버렸을 때 해결하는 예제

- 처음 부트 시에 E키를 눌러서 다음 화면으로 진입해야한다.
![](/assets/img/sample/Linux/ThisisLinux/C4/boot1.JPG)  

- 키보드 아래를 눌러 'linux16 /boot ~' 행에 커서를 놓고 마지막 문장인 'rhgb quiet ~' 를 삭제하고 'init=/bin/sh'를 추가한다.
![](/assets/img/sample/Linux/ThisisLinux/C4/boot2.JPG)  

- ctrl+x 를 눌러 부팅을 하고 sh4-2# 라는 프롬프트가 나올 것이다.
- 'whoami' 명령어를 입력해 현재 사용자가 root인지 확인한다.  

![](/assets/img/sample/Linux/ThisisLinux/C4/boot3.JPG)  

- 'passwd' 명령어를 통해 비밀번호를 바꾼다.  
- /(root) 파티션이 읽기 전용으로 마운트 되어 있어서 권한 오류가 발생할 것이다.
- 'mount -o remount,rw /' 명령어를 입력해 읽기/쓰기 모드로 다시 마운트 시킨다.  
- 'passwd' 명령어를 다시 입력해서 비밀번호를 바꾼다. 
- 재부팅을 하고 새로운 비밀번호를 입력하면 로그인에 성공한다.

-----  




