---
title: 이것이 리눅스다. - 4. 리눅스 명령어, 개념(2)
author: 강민석
date: 2021-01-11 12:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **4.3 리눅스 관리자를 위한 명령어**

### **4.3.1 프로그램 설치를 위한 RPM**

- YUM이 나오기 전에 RPM(Redhat Package Manager)이 사용되었으나, YUM이 RPM의 상위호환이기에 YUM을 사용하면 됩니다.  
- 레드햇사에서 프로그램 설치를 어려워하는 초보자를 위해 setup.exe과 비슷하게 프로그램을 설치한 후에 바로 실행할 수 있는 실행 파일을 제작하였다.  이를 *.rpm이며 이를 '패키지'라고 부릅니다.  

-자주 사용하는 rpm 명령어 옵션은 다음과 같습니다.

```console
rpm -Uvh 패키지파일이름.rpm // 패키지 설치 명령어  
U : 기존에 패키지가 설치되어 있어도 업그레이드를 한다. v : 설치과정 확인 h : 설치과정을 '#'기호로 화면에 출력  

rpm -e 패키지이름 // 패키지 삭제 명령어
e(erase)의 약자 

밑의 둘은 아직 설치되지 않은 rpm 파일 조회
rpm -qlp 패키지이름.rpm // 패키지 파일에 어떤 파일들이 포함되었는지 확인
rpm -qip 패키지이름.rpm // 패키지 파일의 상세 정보
```

![](/assets/img/sample/Linux/ThisisLinux/C4/rpm.JPG)  


- RPM의 단점은 의존성 문제가 있다. Centos의 기본 웹 브라우저인 Firefox는 X 윈도상에서 가동되는데 X 윈도가 설치되지 않았다면 Firefox 
설치될 수 없습니다. 이러한 불편한 점을 해결한 것이 yum이라고 합니다.  

의존성 문제  
![](/assets/img/sample/Linux/ThisisLinux/C4/depen.JPG)  

### **4.3.2 편리하게 패키지를 설치하는 YUM**

- YUM(Yellowdog Updater Modified)은 rpm의 의존성 문제를 해결한 명령어입니다. 
- 의존성이 필요한 패키지들을 찾아서 먼저 설치해주는 명령어입니다. 
- CentOS 프로젝트가 제공하는 rpm 저장소에서 설치할 rpm파일 뿐만아니라 인터넷에서 의존성이 있는 파일을 찾아서 설치합니다.  
- rpm저장소는 /etc/yum.repos.d/에 있습니다.
- 인터넷을 통해 패키지를 설치하므로 인터넷에 연결되어 있어야합니다. 

#### **YUM의 기본 사용법** 
- 기본 설치방법
```console
yum -y install 패키지이름 
-y 옵션은 yes/no를 묻는 부분을 무조건 yes로 입력한것으로 간주 
```

- rpm 파일 설치 방법
```console
yum localinstall rpm파일이름.rpm
기존 rpm과 달리 의존성이 있는 파일을 모두 설치해준다.
```

- 업데이트 가능한 목록보기
```console
yum check-update
패키지 중에서 업데이트가 가능한 목록을 출력한다.
```

- 업데이트
```console
yum update 패키지이름
어차피 yum install을 하면 업데이트기능을 수행하므로 잘 사용하지 않음.
```

- 삭제
```console
yum remove 패키지이름
```

- 정보 확인
```console
yum info 패키지이름
```
- 패키지 그룹 설치
```console
yum groupinstall "패키지그룹이름"
```

- 패키지 리스트 확인
```console
yum list 패키지이름
```

- 특정 파일이 속한 패키지 이름 확인
```console
yum provides 파일이름
```

- GPG 키 검사 생략
```console
yum install --nogpgcheck rpm파일이름.rpm
```

- 기존 저장소 목록 지우기
```console
yum clean all
```

### **4.3.3 파일 압축과 묶기**

**파일 압축**  

- xz  

```console
'파일 이름'을 압축파일인 ~.xz로 만듬 기존파일은 삭제된다.
xz 파일이름

'파일 이름.xz'를 일반 파일인 ~로 만듬. d는 Decompress
xz -d 파일이름.xz

'파일이름.xz' 압축 파일에 포함된 파일 목록과 압축률 등을 출력
l는 list
xz -l 파일이름.xz

압축 후에 기존 파일을 삭제하지 않고 그대로 둔다.
k는 keep
xz -k 파일이름
```
외에도 bzip2, bunzip2(bzip2 압축을 품.), gzip, gunzip(gzip 압축을 품.), zip, unzip이있다. 사용법은 다 비슷합니다.  

**파일 묶기**

- tar  

```console
동작
c -> 새로운 묶음을 만듬.
x -> 묶인 파일을 푼다.
t -> 묶음을 풀기 전에 묶인 경로를 보여준다.
C -> 묶음을 풀 때 지정된 디렉터리에 압축을 푼다. 지정하지 않으면 같은 디렉터리에 푼다.
옵션
f(필수) -> 묶음 파일 이름 지정.
v -> visual의 의미로 파일이 묶이거나 풀리는 과정을 보여줌 
J -> tar + xz
z -> tar + gzip
j -> tar + bzip2

```

### **4.3.4 시스템 설정**  

- 날짜 및 설정
```console
system-config-date //패키지 없다고 뜨면 yum으로 설치
```

- 네트워크 설정
```console
nmtui
```
- 방화벽 설정
```console
firewall-config
```

- 서비스(데몬) 설정
```console
ntsysv
```

- 그 외에 사용되는 설정 명령어
```console
system-config-keyboard
system-config-language
system-config-printer
system-config-users
system-config-kickstart
키보드, 언어, 프린터, 사용자, 킥스타트 설정
```

### **4.3.5 CRONT과 AT**

#### **cron**
- cron은 주기적으로 반복되는일을 자동으로 실행할 수 있도록 시스템 작업을 예약해 놓는 것.
> 00 05 1 * * root cp -r /home /backup  
> 분 시 일 월 요일 사용자 실행명령 *는 매월, 모든 요일을 칭함.  

해석을 하면 매월 1일 새벽 5시 00분에 'cp -r /home /backup' 명령어를 실행한다.

- cron은 주기적으로 실행할 내용을 디렉터리에 넣어 놓고 작동한다.  

![](/assets/img/sample/Linux/ThisisLinux/C4/cron.JPG)  

#### **at**

- at 명령어는 cron과 달리 일회성 작업을 예약하는 것이다.

- 예약 : at 시간
```console
at 3:00am tomorrow
at now + 1 hours
```
- 확인 : at -l
- 취소 : atrm 작업번호







