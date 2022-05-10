---
title: 시작하세요! 도커/쿠버네티스(2) - 도커엔진(1)
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-09 01:00:00 +0800
categories: [Docker,CS]
tags: [Docker]
math: true
mermaid: true
image: 
  path: /assets/img/sample/logo.jpg
comments : true
---

> 시작하세요! 도커/쿠버네티스 책 정리글입니다.
{: .prompt-tip }

# 2. 도커 이미지와 컨테이너

도커의 이미지와 컨테이너는 도커 엔진의 핵심.

## 2.1 도커 이미지

이미지는 기존 가상머신에서 사용한 `iso`파일과 비슷한데, 컨테이너를 생성하고 실행할 때 읽기 전용으로 사용 된다.

이미지의 양식은 다음과 같습니다.

[저장소 이름]/[이미지 이름]:[태그]로 구성되어 있습니다.
```bash
kms/ubuntu:14.04
또는
ubuntu:latest
```

- 저장소 이름(Repository) : 이미지가 저장된 장소. 이미지 생성시 저장소 이름을 명시할 필요가 없으므로 생략하는 경우가 있습니다.
- 이미지 이름 : 해당 이미지가 어떤 역할을 하는지 나타냄. 반드시 설정해야 합니다.
- 태그 : 이미지의 버전 관리, 혹은 리버전(Revision) 관리에 사용 합니다.

## 2.1.2 도커 컨테이너

도커의 이미지는 OS, MySQL같은 DB애플리케이션, 하둡, 스파크 등 다양한 것들이 존재합니다.

이러한 이미지로 컨테이너를 생성하면 해당 이미지의 목적에 맞는 파일이 들어 있는 파일시스템과 격리된 시스템 자원 및 네트워크를 사용할 수 있는 독립된 공간이 생성되고, 이를 `도커 컨테이너`라고 부릅니다.

컨테이너는 이미지를 읽기 전용으로 사용하되 이미지에서 변경 사항은 컨테이너 계층에서 저장하므로 컨테이너에서 무엇을 하든지 원래 이미지에 영향을 주지 않습니다. 컨테이너는 각 독립된 파일시스템을 제공 받으며 호스트와 분리돼 있어서 어떤 애플리케이션을 설치하거나 삭제해도 다른 컨테이너와 호스트 변화가 없습니다.

즉 리눅스 도커 이미지로 두 개의 컨테이너를 생성한 뒤 A 컨테이너에 MySql을, B 컨테이너에 톰캣을 설치해도 각 컨테이너는 영향을 주지 않을뿐더러 호스트에도 영향을 주지 있지 않습니다.

## 2.2 도커 컨테이너 다루기

도커 버전을 확인.

```cmd
docker -v
> Docker version 20.10.14, build a224086
```

첫 번째 컨테이너를 생성합니다.
```cmd
docker run -i -t ubuntu:14.04
```

> -i, -t 옵션은 컨테이너와 상호(interactive) 입출력을 가능하게 합니다. -i는 상호 입출력, -t는 tty를 활성화해서 배시(`bash`)셸을 사용하도록 컨테이너를 설정합니다.
{: .prompt-tip}

![](/assets/img/DockerPost/start_docker_quv/C2/docker%20run.png)

와 같이 `shell`에 변화가 있다면 됩니다.

> 기본 사용자는 `root`이고 호스트 이름은 무작위 16진수 해시값입니다.
{: .prompt-tip}

> 컨테이너를 종료할때 `exit`나 `Ctrl + D`를 쓰지만 이는 컨테이너를 중지시켜버립니다. `Ctrl + P, Q`를 이용하면 컨테이너는 살리고 빠져나오므로 개발시에는 해당 커맨드를 애용합시다.
{: .prompt-tip}

centos:7 이미지를 내려 받기 연습.

```bash
docker pull centos:7
> 7: Pulling from library/centos
6717b8ec66cd: Pull complete 
Digest: sha256:c73f515d06b0fa07bb18d8202035e739a494ce760aa73129f60f4bf2bd22b407
Status: Downloaded newer image for centos:7
docker.io/library/centos:7
```

`docker images`를 통해 존재 확인.

```bash
docker images

REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
centos       7         c9a1fdca3387   2 months ago   301MB
ubuntu       14.04     7304c635fe52   6 months ago   187MB
```

`docker create`를 통해 이미지로 컨테이너 생성

```bash
docker create -i -t --name kmsCentos centos:7
> 233ab6341d888ceab9ee04296c8fb70858bfae8547cbaa0f78a9d4b5391453d2
```

> `--name`옵션을 통해 컨테이너의 이름을 설정할 수 있습니다. 저같은 경우는 'kmsCentos'로 지었습니다.
{: .prompt-tip}

`create` 명령어는 컨테이너를 생성할 뿐 내부로 들어가지는 않습니다.

`docker start`, `docker attach`를 통해 컨테이너를 시작하고 내부로 들어가야 합니다.

```bash
% docker start kmsCentos
> kmsCentos
% docker attach kmsCentos
[root@233ab6341d88 /]# 
```

`docker run` 명령어는 컨테이너를 생성함과 동시에 시작하기에 더 자주 사용 됩니다.

## 2.2.2 컨테이너 목록 확인

```bash
docker ps
kms@kmsui-MacBookPro tomcat_exam % docker ps
> CONTAINER ID   IMAGE      COMMAND       CREATED         STATUS         PORTS     NAMES
233ab6341d88   centos:7   "/bin/bash"   8 minutes ago   Up 6 minutes             kmsCentos
```

정지되지 않은 컨테이너만 출력하기에 `Ctrl + P, Q`로 입력해 빠져나온 `kmsCentos`컨테이너는 목록에 찍히게 됩니다.

> `-a`옵션을 통하여 정지되지 않은 컨테이너도 출력할 수 있습니다.
{: .prompt-tip}

- CONTAINER ID : 컨테이너에게 자동으로 할당되는 고유한 ID, `docker inspect` 명령어를 통해 전체 ID를 확인할 수 있습니다.
- IMAGE : 컨테이너를 생성할 때 사용된 이미지의 이름. 
- COMMAND : 컨테이너가 시작될 때 실행될 명령어.
- CREATED : 컨테이너가 생성되고 난 뒤 흐른 시간
- STATUS : 컨테이너의 상태를 나타냄. 실행중`up`, 종료`Exited`, 일시중지`Pause`등이 있습니다.
- PORTS : 컨테이너가 개방한 포트와 호스트에 연결한 포트. 아무것도 설정하지 않으면 보이지 않습니다.
- NAMES : 컨테이너의 고유한 이름입니다. 지정해줄 수 있고, 지정하지 않는다면 도커 엔진이 임의로 이름을 설정합니다. 
  
> `docker rename` 명령어를 통해 이름을 바꿀 수 있습니다.
{: .prompt-tip}

## 2.2.3 컨테이너 삭제

한 번 삭제한 컨테이너는 복구할 수 없으므로 신중을 기해야합니다.

```bash
docker stop kmsCentos
> 실행중인 컨테이너는 삭제할 수 없으므로 중지 시켜야합니다. `-f`옵션을 통해 강제로 삭제할 수 있습니다.

docker rm kmsCentos
> kmsCentos
```

> `docker container prune` 명령어를 통해 모든 컨테이너를 삭제할 수 있습니다.
{: .prompt-tip}

## 2.2.4 컨테이너를 외부에 노출

컨테이너는 가상 IP주소를 할당 받는데, 도커의 경우는 `172.17.0.x`의 IP를 순차적으로 할당합니다. 

```bash
docker run -i -t --name kms_network_test ubuntu:14.04

root@91816fd6e66d:/# ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:ac:11:00:02  
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:9 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:806 (806.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```


172.17.0.2를 할당받은 eth0 인터페이스와 로컬 호스트인 lo 인터페이스가 있습니다.

아무런 설정을 하지 않으면 외부에서 접근이 불가능 하므로 `eth0`의 IP와 포트를 호스트의 IP와 포트에 바인딩해야 합니다.

```bash
docker run -i -t --name kms_webServer -p 80:80 ubuntu:14.04
> root@f1bfd6065963:/# 
```

`-p`옵션의 [호스트포트]:[컨테이너포트] 이 양식을 통해 포트를 바인딩 해줍니다.

```bash
docker run -i -t -p 8080:8080 -p 192.168.0.1:6666:80 ubuntu:14.04
> -p옵션을 많이 쓸 수 있고, 호스트의 '6666'포트를 컨테이너의 '80'포트와 바인딩 한다는 의미도 내포
```

```bash
root@f1bfd6065963: /# apt-get update
root@f1bfd6065963: /# apt-get install apache2 -y
root@f1bfd6065963: /# service apache2 start
```

이후 [도커 엔진 호스트의 IP]:80 또는 `localhost:80`에 접근하면 아파치 웹 서버에 접근 할 수 있습니다.

아파치 웹 서버는 포트80으로 통신을 하기에 `-p 80:81`처럼 컨테이너 포트를 81로 지정해버리면 웹 서버에 접근하지 못하게 됩니다.

## 2.2.5. 컨테이너 애플리케이션 구축

한 컨테이너에 데이터베이스 + 웹 서버를 넣을 수 있고, 
두 컨테이너로 나눠서 하나는 데이터베이스 하나는 웹 서버로 넣을 수 있겠습니다.

후자의 경우처럼 컨테이너를 구분하는 것이 도커 이미지를 관리하고 컴포넌트의 독립성을 유지하기 쉽습니다. 도커 공식 홈페이지에서도 권장하고 있고, 한 컨테이너에 하나의 프로세스만 실행하는 것이 도커의 철학이기 때문입니다.


### 데이터베이스 + 워드프레스 웹 서버 컨테이너 연동

mac M1인 경우 기존 명령어로 설치가 되지 않기에 명령어를 바꿔야한다.

```bash
# 기존
docker run -d \
--name wordpressdb \
-e MYSQL_ROOT_PASSWORD=password \
-e MYSQL_DATABASE=wordpress \
mysql:5.7

#M1
docker run --platform linux/amd64 \
--name wordpressdb \
-e MYSQL_ROOT_PASSWORD=password \
-e MYSQL_DATABASE=wordpress \
mysql:5.7
```

mySQL 컨테이너를 생성했으므로 워드프레스 웹 서버 컨테이너를 생성합니다.

```bash
docker run -d \
-e WORDPRESS_DB_HOST=mysql \
-e WORDPRESS_DB_USER=root \
-e WORDPRESS_DB_PASSWORD=password \
--name wordpress \
--link wordpressdb:mysql \
-p 80 \
wordpress
```

`-p 80`옵션을 통해 호스트의 임의의 포트와 컨테이너의 80포트를 연결하였습니다.

`docker ps` 명령어를 통해 어느 포트에 연결 되었는지 확인합니다.

```bash
docker ps
> CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                   NAMES
bd85e97e8c0d   wordpress   "docker-entrypoint.s…"   35 seconds ago   Up 33 seconds   0.0.0.0:50953->80/tcp   wordpress
```

50953포트에 연결되었습니다.

> `docekr port wordpress`처럼 docker port <컨테이너 명>을 입력하면 해당 컨테이너가 사용중인 호스트의 포트가 출력 됩니다.
{: .propmt-tip}

![](/assets/img/DockerPost/start_docker_quv/C2/wordpress.png)

`-d`옵션을 넣어서 `run`을 실행하면 입출력이 없는 상태로 컨테이너를 실행합니다. 이렇게 하면 컨테이너 내부에서 프로그램이 터미널을 차지하는 포그라운드로 실행되어, 사용자의 입력을 받지 않습니다.

만약 해당 옵션을 사용하지 않는다면 하나의 터미널을 먹는 `mysqld`이 열리면서 단순히 프로그램이 포그라운드 모드로 동작하는 것만 지켜볼 수 있습니다.

`-e`옵션은 컨테이너 내부의 환경변수를 설정합니다. 

환경변수를 확인하려면 

```bash
echo ${ENVIROMENT_NAME}
```
을 사용하면 됩니다.

새로 만든 wordpressdb 컨테이너에 적용된 환경변수를 출력하려면 2가지 방법이 있습니다.

```bash
#1 exec -t -i 활용 터미널이 살아있음.
docker exec -i -t wordpressdb /bin/bash
root@f95d669519ce:/# echo $MYSQL_ROOT_PASSWORD
password

#2 docker exec 만 활용 결과값만 반환
docker exec wordpress ls /
# 결과값들 리턴
```

실제로 환경변수가 적용되었는지 컨테이너속 mysql로 이동합니다.

```bash
# 이동하는 명령어
docker exec -i -t wordpressdb /bin/bash

# mysql 확인
mysql -u root -p
> Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.7.38 MySQL Community Server (GPL)

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

`--link`옵션은 컨테이너끼리 접근 하는 방법중 하나입니다.

원래는 NAT로 할당받은 IP로 접근할 수 있지만, 무작위성이 강하여 매 번 변경되는 컨테이너의 IP로 접근하기 어렵습니다. 하지만 `--link`옵션은 내부 IP를 알 필요 없이 컨테이너에 별명(alias)으로 접근하도록 설정합니다. 또한, 의존성도 정의해주므로 실행순서도 정해줍니다.

예를들면 위의 경우 `wordpressdb`와 `wordpress` 컨테이너를 중지하고, 다시 `wordpress`를 실행시키면 다음과 같이 에러가 나옵니다.

```bash
docker start wordpress
> Error response from daemon: Cannot link to a non running container: /wordpressdb AS /wordpress/mysql
Error: failed to start containers: wordpress
```

하지만 `--link`옵션은 현재 'deprecated'된 옵션이며 추후 삭제될 가능성이 있습니다. 도커 브리지 네트워크를 사용하면 `--link` 옵션과 동일한 기능을 더욱 손쉽게 사용할 수 있으므로 브리지 네트워크를 사용하는것을 권장한다고 합니다.

