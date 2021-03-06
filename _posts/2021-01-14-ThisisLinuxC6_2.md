---
title: 이것이 리눅스다. - 6. 하드디스크 할당과 사용자별 공간할당(3)
author: 강민석
date: 2021-01-14 11:00:00 +0800
categories: [Linux,ThisisLinux(since2016Stop)]
tags: [This is Linux]
math: true
mermaid: true
image: 
comments: true
---

**한빛 미디어의 이것이 리눅스다. 책을 정리한 자료입니다.**

-----

## **6.3 LVM** ##

- LVM은 Logical Volume Manager의 약자입니다. 논리 하드디스크 관리자라고 할 수 있습니다.
- LVM은 여러 개의 하드디스크를 합쳐서 한 개의 파티션으로 구성한 뒤, 나눌 수 있습니다. 
  2TB 2개를 합쳐 4TB를 만들고 1TB + 3TB로 분리가 가능합니다.
- 물리 불륨(Physical Volume) : /dev/sda1, /dev/sdb1 등의 파티션을 말합니다.
- 불륨 그룹(Volume Group) : 물리 불륨을 합쳐서 1개의 물리 그룹으로 만든 것입니다.
- 논리 불륨(Logical Volume) : 불륨 그룹을 1개 이상으로 나눈 것으로 논리적 그룹이라고 합니다.

![](/assets/img/sample/Linux/ThisisLinux/C6/lvm.png)  

**실습에 관련한 명령어들을 정리하겠습니다.**


```console
pvcreate /dev/sdb1 -> 해당 파티션을 갖고 물리적인 불륨을 생성
vgcreate 불륨그룹명 /dev/sdb1 /dev/sdc1 -> 불륨그룹을 생성하는 명령어
vgdisplay -> 불륨 그룹출력

lvcreate --size 1G --name myLG1 myVG -> myVG에서 1G 사이즈의 논리불륨은 myLG1이라는 이름으로 생성
lvcreate --extents 100%FREE --name myLG2 myVG -> myVG에서 남은 용량을 모두할당한 논리불륨 myLG2를 생성
```

## **6.4 사용자별로 공간 할당하기** ##

리눅스는 여러 명의 사용자가 동시에 접속해서 사용할 수 있습니다. 어떤 사용자가 치명적인 문제를 일으킬 가능성이 있으므로 각 사용자별로 용량을 제한해야 합니다.  
쿼터(Quota)를 정의하면 다음과 같습니다.  
**파일 시스템마다 사용자나 그룹이 생성할 수 있는 파일의 용량과 개수를 제한하는 것**

사용자를 만들고 해당 사용자에게 공간을 할당하는 실습입니다.  

- 1.가상머신에 1GB짜리 하드디스크를 새로 장착합니다.  
![](/assets/img/sample/Linux/ThisisLinux/C6/use.JPG)  

- 2.root로 접속해 /dev/sdb의 파티션 생성과 포맷을 한 후 '/userHome' 디렉터리에 마운트한다.
```console
fdisk /dev/sdb
n
p
1
enter
enter
p
w
mkfs.ext4 /dev/sdb1
mkdir /userHome
mount /dev/sdb1 /userHome
vi /etc/fstab -> 수정해주기
```

- 3.사용자 john과 bann을 만들고 비밀번호도 사용자 이름과 동일하게 설정
```console
useradd -d /userHome/john john
useradd -d /userHome/bann bann
passwd john
passwd bann
```
![](/assets/img/sample/Linux/ThisisLinux/C6/passwd.JPG)  

- 4./etc/fstab 파일을 편집합니다.
```console
vi /etc/fstab
```
![](/assets/img/sample/Linux/ThisisLinux/C6/fstab.JPG)  

- 5.재마운팅 후 확인
```console
mount --options remount /userHome
mount
```
![](/assets/img/sample/Linux/ThisisLinux/C6/mounting.JPG)  

- 6.쿼터를 사용하기 위해 DB생성

```console
cd /userHome
quotaoff -avug -> 쿼터를 일단 끈다.
quotacheck -augmn -> 파일 시스템의 쿼터 관련 체크를 한다.
rm -rf aquota.* -> 생성된 쿼터 관련 파일을 일단 삭제
quotacheck -augmn -> 다시 확인
touch aquota.user aquota.group -> 쿼터 관련 파일을 생성
chmod 600 aquota.* -> 보안을 위해 root 외에는 접근하지 못하게함.
quotacheck -augmn -> 확인
quotaon -avug -> 설정된 쿼터를 시작
ls -l -> 확인
```

- 7.사용자 별로 공간을 할당해줍니다.

```console
edquota -u john -> john사용자의 할당량을 편집
```
![](/assets/img/sample/Linux/ThisisLinux/C6/john.JPG)  

- Filesystem : 사용자별 쿼터를 할당하는 파일 시스템을 의미 /etc/fstab 파일에 /dev/sdb1를 쿼터로 설정했다.
- block,soft,hard : 사용자가 사용하는 블록과 소프트 사용한도, 하드 사용한도 0은 한도를 제한하지 않았다는 것.
- inodes,soft,hard : 파일의갯수 한도를 의미한다. 현재 7개의 파일을 사용하며 제한은 없다는 것을 의미.

다음과 같이 바꿔줍니다.  

![](/assets/img/sample/Linux/ThisisLinux/C6/john2.JPG) 

- 8.john으로 접속하여 확인해봅니다.

![](/assets/img/sample/Linux/ThisisLinux/C6/john3.JPG) 

```console
quota -> 현재 사용자에게 할당된 하드디스크 확인
```
![](/assets/img/sample/Linux/ThisisLinux/C6/quota.JPG) 

>해석 : grace(유예기간)동안 soft로 설정한 10240에서 5120KB을 정리해야한다. (15360KB 모두 사용했으므로)

```console
//root에서 사용해야함.
repquota /userHome -> 사용자별 현재 사용량 확인 가능
```

![](/assets/img/sample/Linux/ThisisLinux/C6/user.JPG) 

- 10.bann사용자에게도 john 사용자와 똑같이 사용량 할당하기

```console
edquota -p john bann -> bann에게도 john과 같은 사항을 적용.
repquota /userHome -> 확인
```

![](/assets/img/sample/Linux/ThisisLinux/C6/result3.JPG) 