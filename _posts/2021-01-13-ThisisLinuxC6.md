---
title: 이것이 리눅스다. - 6. 하드디스크 할당과 사용자별 공간할당(1)
author: 강민석
date: 2021-01-13 14:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **6.1 하드디스크 한 개 추가하기**

### **6.1.1 IDE 장치와 SCSI 장치 구성**

![](/assets/img/sample/Linux/ThisisLinux/C6/ide.JPG)  

현재 서버의 하드디스크 구성 상태입니다.  

IDE0, IDE1에 각각 2개의 IDE장치를 장착할 수 있어 총 4개의 IDE장치를 장착할 수 있습니다.  

일반적으로 사용되는 하드디스크나 CD/DVD 장치가 IDE장치라고 생각하면 됩니다.  
서버용으로는 주로 SCSI 하드디스크를 사용한다고 합니다.  
밑의 사진은 VMware에서 제공하는 SCSI 하드디스크 갯수인데 엄청 많습니다.  
![](/assets/img/sample/Linux/ThisisLinux/C6/hard.PNG)  

처음 장착된 SCSI 하드디스크의 이름을 /dev/sda라고 부릅니다. 그 이후 장착될 때마다 /dev/sdb, /dev/sdc ... 이렇게 부릅니다.  
또한 /dev/sda의 파티션을 나누면 /dev/sda1, /dev/sda2 ... 이렇게 불립니다.  

하드 디스크를 하나 추가하는 예제를 할 것인데, 리눅스는 특정 디렉터리에 마운트를 해야하므로, '/mydata'라는 디렉터리를 만들고 마운트할 것입니다.  

- 1.VMware에서 가상서버를 선택해 [Edit virtual machine settings]에 들어가 HardwareType에서 [Add]를 눌러 모든 설정을 그대로 두고 넘어갑니다. **하드디스크 용량 설정하는 란에서 [Store virtual disk as a single file]를 체크해주셔야합니다.**  

- 2.root권한으로 로그인해주고 VMware 상단에 하드디스크 2개가 장착되어 있는 지 확인합니다.  
![](/assets/img/sample/Linux/ThisisLinux/C6/addhard.JPG)  

- 3.하드디스크에 파티션을 할당해야합니다. 터미널을 키고 다음 명령어들을 입력합니다.

```terminal
fidsk /dev/sdb // SCSI 0:1 하드디스크 선택
n //새로운 파티션 분할 (위의 명령어를 통해 메뉴얼이 나와야합니다.)
p // Primary 파티션 선택
1 // 몇개의 파티션으로 나눌것인지 물어봅니다 1개만 사용할 것이므로 1로 입력합니다.
first sector : Enter // 1개만 사용할 것이므로 디폴트값을 넣습니다.
second sector : Enter // 1개만 사용할 것이므로 디폴트값을 넣습니다.
p // 설정된 내용 확인
w // 설정을 저장합니다.
```

![](/assets/img/sample/Linux/ThisisLinux/C6/addhard2.JPG) 

> 시작섹터가 2048번인 이유는 0~2047부분은 시스템 성능 향상을 위해 사용하지 않는 부분이라고 합니다.

```terminal
mkfs.ext4 /dev/sdb1 //새로운 하드디스크 포맷과 비슷한 과정을 거침.
```

- 4./dev/sdb1 파일 시스템을 사용하기 위해 디렉터리에 마운트해야합니다.  
    과정을 눈으로 보기 위해 임시 디렉터리를 하나 만들어 놓습니다.
    ```terminal
    mkdir /mydata -> 마운트할 디렉터리
    cp anaconda-ks.cfg /mydata/test1 -> 굳이 anaconda-ks 아니여도 아무 임시파일이면 됩니다.
    ls -l /mydata/ -> test1파일이 있는지 확인합니다. 이것이 /dev/sdb1를 마운트하면 잠시 사라질 것입니다.
    ```

    마운트를 하겠습니다.  
    ```terminal
    mount /dev/sdb1 /mydata -> /mydata에 마운트를 합니다.
    ls -l /mydata/ -> lost+found 디렉터리가 있을텐데 무시해도됩니다.
    cp anaconda-ks.cfg /mydata/test2 -> test2파일로 바꿔 복사시킵니다.
    ls -l /mydata/ -> test2가 생겼는데 위에서 생성한 test1이 사라졌습니다.
    ```
    왜냐하면 /mydata는 마운트를 해주는 순간 /dev/sdb1의 소유가 됩니다. 그렇기에 위에서 작성한 test1파일은 잠시 sda2(기존 하드)의 어딘가에 숨어 있습니다. 확인을 위해 마운트를 해제하겠습니다.

    ```terminal
    umount /dev/sdb1 -> 마운트 해제
    ls -l /mydata -> test1파일이 나타났습니다. 마찬가지로 test2파일은 마운트를 다시해주면 생길 것입니다.
    ```

    ![](/assets/img/sample/Linux/ThisisLinux/C6/result1.JPG)  

- 5.컴퓨터를 켤 때 /dev/sdb1 장치가 항상 /mydata에 마운트되도록 설정하겠습니다.

```terminal
vi /etc/fstab // 파일을 엽니다. 밑과같이 맨 밑줄에 설정을 하나 추가합니다.
```
![](/assets/img/sample/Linux/ThisisLinux/C6/addhard3.JPG)  

- 설정 파일의 내용은 다음과 같습니다.
    + 장치이름, 마운트될 디렉터리, 속성, dump 사용여부, 파일 시스템 체크 여부
    + 파일 시스템과 속성을 defaults로 설정 시 읽기/쓰기/실행 등의 작업이 대부분 가능
    + dump를 1로 설정하면 dump 명령어를 이용한 백업이 가능합니다.
    + 체크 시스템의 숫자는 2인경우 1인 파일시스템을 먼저 체크하고 체크하게 되는 순서를 입력합니다. 0인경우 체크하지 않아 부팅이 빠릅니다.

설정을 저장한 후 'reboot'로 재부팅을 해준 뒤 'ls -l /mydata' 명령어를 통해 test2가 있는지 확인합니다.  

![](/assets/img/sample/Linux/ThisisLinux/C6/result2.JPG)  

    
