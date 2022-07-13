---
title: Docker - 도커 볼륨 
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-07-13 00:00:00 +0800
categories: [Docker,CS]
tags: [Docker]
math: true
mermaid: true
image: 
  path: /assets/img/DockerPost/docker.png
comments : true
---

> 시작하세요! 도커/쿠버네티스 책 정리글입니다.
{: .prompt-tip }

# ✍️ 도커 볼륨

도커 이미지로 컨테이너를 생성하면 이미지는 읽기전용이 된다고 합니다.

컨테이너의 변경이력이나 정보는 별도로 저장하는데, 컨테이너가 관리 합니다.

만약에 `mysql`와 `wordpress`를 함께 쓰는 도커가 있다고 가정하면 `mysql`은 `wordpress`에서 작성한 정보들을 가지고 있을 것인데 그 정보들은 컨테이너에서 가지고 있고, 이미지는 `mysql`을 실행하기 위한 정보만을 가지고 있다고 합니다.

문제는 `mysql`컨테이너를 삭제하면 컨테이너에서 가지고 있던 정보가 삭제 된다는 것 입니다.

한 번 삭제하면 데이터 복구가 어려운 관계로 컨테이너를 삭제하지 않기 위해 컨테이너의 데이터를 **영속적** 데이터로 활용할 수 있는 방법이 몇 가지 방법을 제공하고 있습니다.

가장 활용하기 쉬운 방법이 **볼륨**을 사용하는 것.

- 호스트와 볼륨을 공유
- 볼륨 컨테이너를 활용
- 도커가 관리하는 볼륨을 생성

위 방법들을 활용하여 컨테이너를 삭제해도 데이터는 삭제되지 않도록 설정할 수 있다고 합니다.

처음부터 차근차근히 알아보겠습니다.

# ☝️ 1. 호스트 볼륨 공유


M1칩 기준 다음 명령어로 mysql 데이터베이스 컨테이너와 워드프레스 웹 서버 컨테이너를 생성하겠습니다.

```bash
# mysql M1
docker run -d \ 
--name wordpressdb_hostvolume \
-e MYSQL_ROOT_PASSWORD=password \
-e MYSQL_DATABASE=wordpress \
-v /tmp/wordpress_db:/var/lib/mysql \
mysql:5.7
```

```bash
# wordpress
docker run -d \
-e WORDPRESS_DB_HOST=mysql \
-e WORDPRESS_DB_USER=root \
-e WORDPRESS_DB_PASSWORD=password \
--name wordpress_hostvolume \
--link wordpressdb_hostvolume:mysql \
-p 80 \
wordpress
```

- `-v`옵션은 호스트의 `/tmp/wordpress_db` 디렉터리와  컨테이너의 `var/lib/mysql` 디렉터리를 공유한다는 뜻입니다.

그러면 `/tmp/wordpress_db`에는 다음과 같이 나오게 됩니다.

![](/assets/img/DockerPost/start_docker_quv/C2/volume1.png)

> `var/lib/mysql`은 `mysql`이 데이터베이스의 데이터를 저장하는 기본 디렉터리입니다.
{: .prompt-tip }

만약 컨테이너에 파일들이 존재하는데 이를 공유하는 시도를 한다면
호스트 디렉터리에 있는 파일들이 엎어씌워집니다.  `-v`옵션을 통한 호스트 볼륨 공유는 호스트의 디렉터리를 컨테이너의 디렉터리에 마운트 합니다.

# ☝️ 2. 볼륨 컨테이너

`-v` 옵션으로 볼륨을 사용하는 컨테이너를 다른 컨테이너와 공유하는 방법입니다.

컨테이너를 생성할 때 `--volumes-from`옵션을 지정하면 `-v`, `--volume`옵션을 적용한 컨테이너의 볼륨과 공유할 수 있습니다. 

예를들어서 `volume_overide`라는 컨테이너는 위에서 작성한 `/tmp/wordpress_db`의 내용을 `/home/testdir_2`에 공유한다고 하면

```shell
docker run -i -t \
> --name volumes_from_container \
> --volumes-from volume_overide \
> ubuntu:14.04
```

명령어를 통해 `voumes_from_container` 컨테이너도 공유받을 수 있습니다.

![](/assets/img/DockerPost/start_docker_quv/C2/volume2.png)

이를 통해서 여러 개의 컨테이너가 동일한 컨테이너에 볼륨을 공유해 사용할 수 있습니다.

이러한 구조를 통해서 호스트에서 볼륨만 공유하고 별도의 역할은 각 컨테이너에 맡길 수 있습니다.

# ☝️ 3. 도커 볼륨

도커 자체가 제공하는 볼륨 기능입니다.

`docker volume`으로 시작하며 `docker volume create`로 볼륨을 생성할 수 있습니다.

```shell
docker volume create --name myvolume
```

`myvolume`이라는 볼륨을 생성했고, 다음 명령을 통해 확인할 수 있습니다.

```shell
# docker volume ls
DRIVER VOLUME NAME
local   myvolume
```
해당 볼륨은 로컬호스트에 저장되며 도커 엔진에 의해 생성되고 삭제될 수 있습니다.

그 다음 `myvolume`을 사용하는 컨테이너를 생성해야 합니다.

```shell
docker run -i -t --name myvolume_1 \
-v myvolume:/root/ \
ubuntu:14.04

# 접속 후
echo hello,volume >> /root/volume
```

이러면 `/root/volume`에 "hello,volume"이 쓰이게 됩니다.

그리고 `/root`에 쓰이는 모든 것들은 `myvolume`에 같이 공유될 것입니다.

이를 확인하기 위해 다른 컨테이너를 생성해 봐야합니다.

```shell
docker run -i -t --name myvolume_2 \
-v myvolume:/root/ \
ubuntu:14.04

# 접속 후
cat /root/volume

# 결과
hello,volume
```
실제 `volume`파일을 생성하지 않아도 컨테이너에 존재해 있고 확인도 가능합니다.

이를 통해서 도커 볼륨도 여러 개의 컨테이너에 공유되어 활용될 수 있습니다.

----- 

볼륨은 디렉터리 단위로서 도커 엔진에서 관리합니다.

도커볼륨도 호스트 볼륨과 마찬가지로 호스트에 데이터를 보존하지만 파일이 실제 어디에 저장되는지는 사용자는 알 필요가 없다고 합니다.

`docker inspect`명령어를 통해 실제 도커 볼륨이 어디에 저장되는지 알 수 있습니다.


![](/assets/img/DockerPost/start_docker_quv/C2/volumeSave.png)

> `type`을 사용한 것은 볼륨에 대한 정보를 알고 싶음을 명시적으로 표현한 것입니다.
{: .prompt-tip }

> 뿐만 아니라 `docker container inspect <cotainer name>`, `docker volume inspect <volume>`와 같이 원하는 정보를 출력할 수 있습니다.
{: .prompt-tip }

> 하지만 mac에서는 `docker` 동작방식이 달라 이 방식으로 확인이 불가능 합니다.
{: .prompt-warn }

-----

매 번 `docker volume create`를 사용해 볼륨을 생성하고 공유하는 것은 번거로울 수 있습니다.

때문에 `-v` 옵션을 입력할 때 이를 수행하도록 설정할 수 있습니다.

```shell
docker run -i -t --name volume_auto \
-v /root \
ubuntu:14.04
```

이렇게 하면 자동으로 이름을 정해서 볼륨을 만들어 줍니다.

```shell
# docker volume ls
DRIVER  VOLUMENAME
local     d0b67fd9ece36c5296d022a46942a0ded6f862495bdf2c24302ab75f485f3640
local     myvolume
```

대신 16진수로 들어가게 됩니다.

때문에 컨테이너가 해당 볼륨을 확인하기 위해서는 `docker container insepect`가 필요합니다.

```shell
docker container inspect volume_auto
```

![](/assets/img/DockerPost/start_docker_quv/C2/volumeDetail.png)

도커 볼륨을 생성하고 삭제하다보면 불필요한 볼륨이 남아있을 때가 있습니다.

도커 볼륨을 사용하고 있는 컨테이너를 삭제해도 볼륨은 삭제되지 않기 때문입니다. 

사용되지 않는 볼륨을 한 번에 삭제하려면 `docker volume prune`를 사용합니다.

```shell
docker volume prune
```

![](/assets/img/DockerPost/start_docker_quv/C2/volumePrune.png)

위의 방식들처럼 컨테이너가 아닌 외부에 데이터를 저장하고 컨테이너는 그 데이터로 동작하게끔 설계하는 것을 `stateless`하다고 말합니다.

컨테이너는 상태[데이터]를 외부로부터 주입받고 컨테이너가 삭제되어도 데이터는 보존되므로 이러한 컨테이너 설계는 도커를 사용할 때 매우 바람직한 설계라고 합니다.

이와 반대로 컨테이너가 상태[데이터]를 자체적으로 저장하고 있는 경우 `stateful`하다고 합니다. 이러한 설계는 **지양**하는 편이 좋습니다.

> `-v`옵션 대신 `--mount` 옵션을 사용할 수 있습니다. 구문이 좀 더 명시적입니다. `docker run -i -t --name mount_option_1 \ 
--mount type=volume, source=myvolume, target=/root \
ubuntu:14.04`
호스트의 디렉터리를 컨테이너 내부에 마운트하는 경우 type에 `bind`를, source에는 호스트의 디렉터리 경로를 지정해야 합니다.
{: .prompt-tip }











