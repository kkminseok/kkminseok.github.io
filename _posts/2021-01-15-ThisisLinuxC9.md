---
title: 이것이 리눅스다. - 9. 네임 서버 설치와 운영 (중지)
author: 강민석
date: 2021-01-15 12:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **9.1 네임 서버의 개념** ##

- 네임 서버는 DNS(Domain Name System) 서버라고도 하며 URL을 IP주소로 변환하는 일을 하는 서버입니다.
- 옛날에는 hosts 파일을 가지고 IP를 알았다고 합니다. 호스트파일을 열어보겠습니다. 

Server에서 root권한으로 다음 명령어를 입력합니다.  
```console
nslookup
www.nate.com
www.naver.com  
등등.. 보고싶은 URL
```

![](/assets/img/sample/Linux/ThisisLinux/C9/dns.JPG)  

자신의 컴퓨터에 네임서버를 작동시키지 않아도
위에서 나온 IP를 주소창에 입력하면 해당 페이지가 나옵니다.  

```console
vi /etc/resolv.conf
에서 nameserver ~ 부분 '#'으로 주석처리
```

그 이후 인터넷에 'naver.com'을 입력하면 찾을 수 없다고 나옵니다.  

![](/assets/img/sample/Linux/ThisisLinux/C9/dns2.JPG)  

하지만 위에서 찾은 IP주소를 넣어주면 접속이 됩니다.  
라는데.. 저는 잘 안됩니다. 구글링해도 잘 안나오길래 일단 넘어가고 진행했습니다.   

**naver는 안되는 것 같습니다. 저희 학교인 '163.180.96.211'은 잘됩니다.. 왜지?..**

위의 상태에서 /etc/hosts 파일을 이용해서 IP입력 말고 URL 입력으로 들어가는 예제입니다.  
이 예제는 잘 됩니다.

```console
vi /etc/hosts
```
![](/assets/img/sample/Linux/ThisisLinux/C9/dns3.JPG)  

**nate.com의 주소는 각자 다를 수 있습니다.**
위처럼 수정하고 주소창에 'www.nate.com'을 검색하면 잘 나옵니다.  

![](/assets/img/sample/Linux/ThisisLinux/C9/nate.JPG)  

만약 위의 파일을 밑처럼 바꾸면 경희대로 갈 수도 있습니다.

![](/assets/img/sample/Linux/ThisisLinux/C9/khu.JPG)  
![](/assets/img/sample/Linux/ThisisLinux/C9/khu2.JPG)  
<br>
IP주소를 얻는 흐름도는 다음과 같습니다.

![](/assets/img/sample/Linux/ThisisLinux/C9/ip.PNG)  

- /etc/resolve.conf 보다 /etc/hosts가 우선순위에 있다는 것입니다.  그렇기에 hosts에 저장된 URL만 입력해도 홈페이지가 나타는 것입니다.
- 네임서버가 있어야하는 이유는 제가 실습하면서 저 위의 IP들을 많이 쓰면서 외워버렸지만, 처음에 ip주소를 찾는 번거로움이 너무 답답했습니다. 그 때문에 바로바로 전환을 도와주는 서버가 있어야 한다는게 제 생각입니다.

-----  

## **9.2 네임 서버의 구축** ##

### **9.2.1 도메인 이름 체계** ###

인터넷이 보급되지 않았을 때 1대의 네임 서버만으로도 IP주소를 알 수 있었습니다. 하지만 인터넷이 폭발적으로 확장되기 시작했고, 1대로 관리하기 어려워졌습니다. 그래서 트리 구조 형태의 도메인 이름 체계가 고안되었습니다.  

![](/assets/img/sample/Linux/ThisisLinux/C9/dnsname.jpg)  

위의 사진에서 1단계 네임 서버는 net, com, org, ... 등이 있는데 com 네임서버는 그 하위 자식들인 nate, google, naver만 관리하면 됩니다. 

### **9.2.2 로컬 네임 서버가 작동하는 순서** ###

리눅스에는 위에서 살펴본 /etc/resolv.conf 파일에 'nameserver IP주소' 형식으로 설정되어 있습니다.  
이 네임 서버를 로컬 네임 서버라고 부르는데, 이 로컬 네임 서버가 전세계의 도메인을 관리할 수 없기 때문에 다른 네임서버에 요청을 보냅니다.  

![](/assets/img/sample/Linux/ThisisLinux/C9/local.jpg)
> 출처: https://wikidocs.net/9668 위의 사진도 잘 안보이시면 여기서 찾으시면 됩니다.

www.nate.com를 입력했을 때를 가정하면 순서는 다음과 같습니다.
1. www.nate.com 입력
2. 리눅스 경우 /etc/resolv.conf에서 'nameserver 네임서버ip'부분을 찾아 로컬 네임 서버 컴퓨터를 알아냄
3. 로컬 네임 서버에 위의 주소를 물어봄
4. 자신의 캐시 DB에 있으면 알려주지만 없으면 'ROOT 네임 서버'에 물어봄.
5. 'ROOT 네임 서버'도 모르므로 'com 네임 서버'의 주소를 알려주면서 거기에 물어보라고 함.
6. 'com 네임 서버'도 모르므로 'nate.com'을 관리하는 네임 서버의 주소를 알려주면서 물어보라고 함.
7. 'nate.com 네임 서버'는 네이트에서 구축한 네임 서버로 ip주소를 알고 있어서 알려줌.
8. pc는 받은 ip로 접속을 시도함.

### **9.2.3 캐싱 전용 네임 서버** ###

Cash Only Nameserver라고 불리는 이 서버는 URL로 IP 주소를 얻고자 할 때, 해당하는 URL의 IP 주소를 알려주는 네임 서버를 말합니다.  

**네임 서버 구축 예제가 있는데 책 집필 시간이 지나 패키지 버전 오류로 인해 진행할 수 없습니다. 읽고 넘어갔습니다.**

해결 법을 위해 재설치 등 방법을 사용해보았으나 결과적으로 
```console
yum -y install bind bind-chroot
```
의 문제점은 제가 가지고 있는 패키지 버전이 너무 높아서 의존성에 의해 위의 패키지를 다운받을 수 없다고 합니다.  
그 패키지는 CentOS가 설치될 때 자동으로 설치되는 것으로 CentOS7의 부 버전이 너무 높은건가 싶어서 CentOS7중 버전이 낮은 것을 따로 다시 설치해야할 것 같아서 시간이 좀 걸릴 것 같습니다.  


### **9.2.4 마스터 네임 서버** ###

- 자신이 별도로 관리하는 도메인이 있으며, 외부에서 자신이 관리하는 컴퓨터의 IP 주소를 물어볼 때, 자신의 DB에서 찾아서 알려주는 네임 서버를 '마스터 네임 서버'라고 부릅니다.

### **9.2.5 라운드 로빈 방식의 네임 서버** ###

- 요청 많은 서버는 웹 서버를 1대가 아니고 여러 대가 있을텐데 요청이 들어올 때 교대로 처리하는 방식을 라운드 로빈방식이라고 합니다.



**버전 차이가 나서 극복 하려고 했으나 잘 되지 않습니다. 책도 절판인데다가 새로운 버전의 책이 동일한 방식으로 계속 나오고 있고 카페를 둘러보니 재설치 외에 방법은 없다고 하니 다음에 리눅스 공부할 기회가 있을 때 해야할 것 같습니다.**