---
title: 이것이 리눅스다. - 5. X 윈도 사용법
author: 강민석
date: 2021-01-12 15:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **5.1 그놈 데스크톱 환경 설정** ##

- #### **X 윈도 바탕화면 변경하기** ####
    바탕화면에서 마우스 오른쪽 키를 눌러 [바탕화면 배경 바꾸기]를 선택합니다.  
    또는 firefox에 들어가 <http://gnome-look.org>에서 마음에 드는 배경을 다운받아서 적용시킵티다.  
    나같은 경우는 블로그 메인 고양이 사진을 배경화면으로 바꾸었습니다.  
    ![](/assets/img/sample/Linux/ThisisLinux/C5/background.JPG)  
<br>

- #### **테마  변경하기** ####

    먼저 'rpm -qa gnome-tweak-tool' 명령어를 통해 패키지가 설치되어있는지 확인합니다.  
    확인을 한 뒤 설치가 되어있지 않다면 'su -c "yum -y install gnome-tweak-tool"' 명령어를 통해 관리자 권한으로 패키지 설치를 수행합니다.  
    'gnome-tweak-tool' 명령어를 통해 켜야한다고 책에 나와있지만, 현재는 버전이 바뀌어서 'gnome-tweaks'명령어를 쳐야합니다.  
    gnome-tweaks로 통합되었다고 합니다.  
    참고 : <https://wnw1005.tistory.com/44>   
    그 다음 테마 탭이나 모양새 탭에 들어가 원하는 테마로 바꾸주면 됩니다.  
    ![](/assets/img/sample/Linux/ThisisLinux/C5/theme.JPG)  
<br>

- #### **GURB 화면 바꾸기** ####
    >책에 있는 순서대로 안됩니다. 이유는 boot/grub2에 theme 폴더에 관련 자료를 넘거야하는데 theme 폴더가 없습니다.  

    ![](/assets/img/sample/Linux/ThisisLinux/C5/error.JPG)    
    보아하니 이것 또한 세월이 지나면서 패키지명이 바뀌거나 그런것 같은데 찾아봐도 내용이 없어서 다음 블로그를 참고하여 설치해줬습니다.  
    <https://powerade-kim.blogspot.com/2018/11/>  
    설치해주니 없던 theme 폴더가 생겼습니다.
    
    먼저 위의 링크를 참고해 패키지를 다운 받습니다.
    ```console
    su -c 'gedit /etc/default/grub'
    ```
    를 통해 문서를 열고 맨 밑에 'GRUB_THEME="/boot/grub2/themes/system/theme.txt"를 입력해 부트 시 참고해야할 문서경로를 적어줍니다.  
    그리고 GRUB_TERMINAL_OUTPUT="console"은 '#'을 이용해 주석처리합니다.  
    자신이 원하는 이미지를 인터넷에서 다운받고 다음 명령어들을 쳐줍니다.  
    ```console
    cd ~/다운로드/ -> 기본적으로 다운받은 파일이 저장되는 경로
    pwd -> 맞게왔는지 확인
    ls -l -> 자신이 다운받은 파일명과 확장자를 기억합니다.
    su -c 'mv 자신의파일명.확장자 /boot/grub2/themes/system/' -> 뒤의 폴더에 자신이 다운받은 파일을 옮깁니다.
    su -c 'gedit /boot/grub2/themes/system/theme.txt' -> theme.txt 파일을 엽니다.
    ```

    theme.txt 파일에서 밑에 내리다보면 desktop-image : "firefox.png"를 자신의 파일명으로 바꿔줍니다.  

    ```console
    su -c 'grub2-mkconfig -o /boot/grub2/grub.cfg' -> grub.cfg를 변경합니다.
    reboot -> 재부팅해서 확인합니다.
    ```
![결과](/assets/img/sample/Linux/ThisisLinux/C5/theme2.JPG)  

-----

## **5.2 X 윈도 응용프로그램** ##

### **5.2.1 파일브라우저 - 노틸러스** ###

- 노틸러스는 그놈 데스크톱 환경에서 제공되는 파일관리자
- windows의 '파일 탐색기'와 비슷
- 사용방법은 프로그램 -> 보조프로그램 -> 파일 선택 or 터미널에 'nautilus' 명령어 입력

### **5.2.2 인터넷 응용프로그램** ###

- Firefox (최신 버전으로 업그레이드하는 예제가 있지만 저는 이미 최신버전이라 생략)
- 메일 클라이언트 - 에볼루션 -> 터미널에 'evolution' 명령어 입력 or [프로그램] -> [오피스] -> [에볼루션]
                            -> 저는 없어서 'su -c "yum -y install evolution"'으로 설치해줬습니다.
- 메신저 - 엠퍼시 -> 엠퍼시는 대부분의 메신저 서비스 프로그램을 사용할 수 있다. 
                 -> [프로그램] -> [인터넷] -> [엠퍼시] or 'empathy' 명령어 입력
- FTP 클라이언트 - gFTP -> CentOs7에서 제공하지 않으므로 다른 리눅스의 패키지를 가져와 설치해야한다.

### **5.2.3 그 외 명령어들** ###

```console
rhythmbox -> 라디오
totem -> 동영상 플레이어
gedit -> 텍스트 편집기
evince -> PDF,XPS 등이 다중 문서를 볼 수 있는 뷰어 프로그램
brasero -> CD/DVD 레코딩
gimp -> 그래픽 편집 프로그램
eog -> 그림 보기
gnomoe-screenshot -> 화면 캡쳐
libreoffice -> MSoffice와 비슷한 것 호환성이 뛰어남
libreoffice --writer -> 워드 프로세서
libreoffice --calc -> 스프레드시트
libreoffice --impress -> 프로제테이션 툴
```

-----  

## **5.3 KDE 환경 사용해보기** ##

```console
yum grouplist | grep KDE -> KDE 그룹 관련 리스트 확인
su -c 'yum -y groupinstall "KDE Plasma Workspaces" -> 그룹 패키지 설치
reboot -> 재부팅
```

재부팅을 하고 로그인을 하기 전 톱니바퀴를 눌러줘서 KDE Plasma 작업공간으로 바꿔주면 KDE를 사용하게 됩니다.  

![](/assets/img/sample/Linux/ThisisLinux/C5/KDE.JPG)  



