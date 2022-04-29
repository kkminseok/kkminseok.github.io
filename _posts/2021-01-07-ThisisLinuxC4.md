---
title: 이것이 리눅스다. - 4. 리눅스 명령어, 개념(1)
author: 강민석
date: 2021-01-07 22:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **4.1 시작과 종료**

터미널/콘솔에서 시스템 종료 명령 실행하는 법이 있습니다.  
- poweroff / shutdown -P now/ halt -p / init ()    -P, -p 는 시스템 종료를 의미한다.
```console
shutdown -P +10 // 10분 후에 종료 (P:powerOff)
shutdown -r 22:00 // 오후 10시에 재부팅(r:reboot)
shutdown -c // 예약된 shutdown을 취소 (c: cancel)
shutdown -k +15 // 현재 접속한 사용자에게 15분 후에 종료된다는 메시지를 보내지만 실제로 종료는 안됨.
```

- 재부팅 명령어

```console
shutdown -r now
reboot
init 6
```

- 로그아웃 명령어
```console
logout
exit
```

-----  

### **4.1.1 가상콘솔 사용하기**
centos는 총 6개의 가상 콘솔을 제공한다. X 윈도가 1번째이고 2~6번째는 텍스트모드로 제공된다.  
키는 'Ctrl + Alt + F1~F6 ' 이다.  

- 런레벨
런레벨은 init 명령어 뒤에 붙는 숫자를 런레벨이라고 한다.

|런레벨|영문모드|설명|비고|
|:---|:---:|---:|
|0|Power Off|종료모드|
|1|Rescue|시스템 복구 모드|단일 사용자모드|
|2|Muti-User||사용하지 않음|
|3|Muti-User|텍스트 모드의 다중 사용자 모드||
|4|Muti-User||사용하지않음|
|5|Graphical|그래픽 모드의 다중 사용자 모드||
|6|Reboot|||

런레벨 모드 확인하려면 /lib/systemd/system 디렉터리의 runlevel?.target 파일을 확인한다.(링크파일이다.)  
![](/assets/img/sample/Linux/ThisisLinux/C4/runlevel.JPG)  

-----  

### **4.1.2 부팅 시에 텍스트 모드로 부팅되도록 런레벨을 변경시키기**
```console
ls -l /etc/systemd/system/default.target
```
이 명령어를 사용하면
~ system/graphical.target 이렇게 나와있는데, 현재 그래픽모드로 부팅이된다는 것이다.

텍스트 모드로 부팅되도록 런레벨을 바꾸려면 다음 명령어를 치면 된다.
```console
ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target
ls -l /etc/systemd/system/default.target
reboot // 재접속
```

-----  

### **4.1.3 텍스트모드를 그래픽 모드로 변경시키기**

```console
ln -sf /lib/system/graphical.target /etc/systemd/system/default.target
reboot //재접속
```

-----  

### **4.1.4 자동 완성과 히스토리 사용하기**

```console
history // 사용했던 명령어를 볼 수 있다.
history -c // 기억되었던 명령어를 삭제한다.
```
밑은 history를 쓰고 다시 history -c를 통해 기억되었던 명령어를 삭제시킨 것이다.  
![](/assets/img/sample/Linux/ThisisLinux/C4/history.JPG)  

자동완성은 Tab 키를 눌러 확인할 수 있다. 비슷한 이름이 많을 경우 Tab 키를 두 번 누르면 된다.  

-----

### **4.1.5 vi 편집기 사용하기**

```console
vi //vi편집기 키기
```
esc를 누르면 나오는 것은 'ex 모드', '라인 명령 모드'라 부른다.  
일반모드는 '명령 모드'라고 한다. 이 상태에서 'a'(append), 'I'(insert) 키를 누르면 '입력 모드'로 바뀐다.
'wq!'는 저장(Write)하고 종료(Quit)된다.  
<br>

만약 수정 작업중 정상적으로 종료되지 않았으면 .swp 파일이 생겼을 것이다.
```console
rm -f 파일명 //으로 삭제하자.
```
![](/assets/img/sample/Linux/ThisisLinux/C4/swp.JPG)  
```
vi 파일명 
```

오류를 무시하고 spacebar를 두 번 누르고 ':q!' 로 나온다.  
임시파일이 만들어진 것은 전의 작업이 제대로 종료되지 않았을 경우 나타나는 것이다.  

ex 모드에서 문자열을 치환하려면 '%s/기존문자열/새문자열'형식으로 사용해야 한다.
```console
:%s/centos/linux //centos를 linux로 바꿈.
```
![](/assets/img/sample/Linux/ThisisLinux/C4/s1.JPG){: width= "300" height = "300"}  
![](/assets/img/sample/Linux/ThisisLinux/C4/s2.JPG){: width= "300" height = "300"}  


### **4.1.6 마운트와 CD/DVD/USB의 활용**

cd나 dvd를 잘 사용하지 않으므로 간단히 봤습니다.  

ISO 파일을 생성하는 명령어는 'genisoimage'이다.  
ISO 파일을 cd로 굽는 명령어는 'cdrecord'이다.
ISO 파일을 dvd로 굽는 명령어는 'growisofs'이다.

다음 명령어로 위의 명령어를 사용하기 위한 패키지가 설치되어있는 지 확인할 수 있다.  
설치가 되어있지 않다면 아무것도 출력되지 않는다.  
```console
rpm -qa genisoimage
rpm -qa wodim
rpm -qa dvd+rw-tools  

yum -y install 패키지명 으로 설치가 가능하다.
```

-----  

## **4.2 사용자 관리와 파일 속성** ##

### **4.2.1 사용자와 그룹** ###

사용자 확인하는 명령어
```console
vi /etc/passwd
gedit /etc/passwd
```

![](/assets/img/sample/Linux/ThisisLinux/C4/user.JPG)  

> 사용자 이름 : 암호 : 사용자 ID : 사용자가 소속된 그룹 ID : 전체 이름 : 홈 디렉터리 : 기본 쉘
> 암호가 x로 표시되어 있는 것은 /etc/shadow 파일에 비밀번호가 저장되어 있다는 뜻. 

그룹 확인하는 명령어
```console
vi /etc/group
```

![](/assets/img/sample/Linux/ThisisLinux/C4/group.JPG)

> 그룹 이름 : 비밀번호 : 그룹 ID : 그룹에 속한 사용자 이름

사용자 및 그룹과 관련된 명령어

```console
useradd [옵션] 사용자 이름 // 새로운 사용자 추가 /etc/password,/shadow,/group파일에 새로운 행 생김.
passwd 사용자 이름 // 사용자의 비밀번호를 지정하거나 변경
usermod [옵션] 사용자 이름 // 사용자의 속성을 변경
usedel [옵션] 사용자 이름 // 사용자 삭제
chage [옵션] 사용자 이름 // 사용자의 암호를 주기적으로 변경하도록 설정.
groups [사용자이름] // 사용자가 소속된 그룹을 보여준다.
groupadd [옵션] 그룹이름 // 새로운 그룹을 생성한다.
groupmod [옵션] 그룹이름 // 그룹의 속성을 변경한다.
groupdel 그룹이름 // 그룹을 삭제한다.
gpasswd [옵션] 그룹이름 // 그룹의 암호를 설정하거나 그룹 관리를 수행한다.
```

'/etc/skel'는 사용자가 생성될 때 해당 디렉토리의 내용을 사용자폴더에 복사시킴.  
사용자가 생성될 때마다 배포를 하기원하는 파일이 있으면 skel 디렉터리에 파일을 넣어놓으면 된다.  

'yum -y install system-config-users'를 통해 해당 패키지를 설치 후 'system-config-users-' 명령어를 입력하면 X 윈도에서 사용자를 쉽게 관리할 수 있는 UI를 제공한다.  

### **4.2.2 파일 디렉토리의 소유와 허가권** ###

![](/assets/img/sample/Linux/ThisisLinux/C4/file.JPG)  
![](/assets/img/sample/Linux/ThisisLinux/C4/fileoption.JPG)  

**파일 유형**

디렉토리 : d  
일반적인 파일 : -  
블록 디바이스 : b -> 하드디스크, 플로피 디스크, CD 등  
문자 디바이스 : c -> 마우스, 키보드, 프린터 등  
링크 : l -> window의 바로 가기 아이콘과 비슷한 역할로 연결된 파일을 의미. 실제 파일은 다른 곳에 존재함.  

**파일 허가권**

읽기 : r (read)  
쓰기 : w (write)  
실행 : x (execute)  
위의 예제에서 뿐만아니라 'rw-', 'r--', 'r--' 이런 식으로 3개씩 끊어서 봐야한다.  
첫 번째는 소유자(User) 권한, 두 번째는 그룹(Group) 권한, 세 번째는 그 외 사용자(Other) 권한이다.  
'rw-'에서  
첫 번째 칸은 '4' 두 번째칸은 '2' 세 번째 칸은 '1'로 표현되며 'rw-'는 6으로 표현될 수 있다.
'rwx'인 경우는 4+2+1로 7로 표현된다.  
위의 예제는 소유자는 읽고 쓸 수 있으며 그룹과 다른사용자는 읽기만 가능하다. 라는 뜻이다.  


**파일 소유권**

파일 소유권은 파일을 소유한 사용자와 그룹을 의미한다.  
'chown' 명령어를 통해 바꿀 수 있다.
```console
chown centos sample.txt // sample.txt 파일의 소유자를 centos로 바꾸라는 의미
chwon centos.centos sample.txt // sample.txt 파일의 그룹도 centos 그룹으로 바꾸라는 의미.
chgrp centos sample.txt // 그룹만 centos로 바꾸라는 명령어
```

**링크**

하드 링크 파일과 심볼릭 링크파일이라는 두 가지 종류의 링크파일이 있다.  

![](/assets/img/sample/Linux/ThisisLinux/C4/link.JPG)  
이해를 돕기위한 간단한 예제가 있다.
```console
vi basefile // basefile 을 만들고 안에 아무 내용이나 넣고 저장하고 끄자.
ln basefile hardlink // basefile을 기반한 hardlink파일을 만든다.
ln -s basefile softlink // basefile을 기반한 softlink파일을 만든다.
ls -il // -il 옵션을 통해 inode 번호를 제일 앞에 출력한다.
cat hardlink // 하드 링크 내용을 확인 
cat softlink // 소프트 링크 내용을 확인
```
![](/assets/img/sample/Linux/ThisisLinux/C4/result.JPG)  

위의 결과를 보면 hardlink파일과 basefile의 inode값은 같다. softlink는 일종의 포인터라 생각하면 쉽다.  
만약 softlink 파일과 원본 파일이 다른 디렉토리에 위치하게 되면 연결이 끊어져 열리지 않는다.  




