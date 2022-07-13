---
title: Docker - 도커 네트워크
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-07-13 03:00:00 +0800
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

# 📖 도커 네트워크

컨테이너 내부에서 `ifconfig`을 입력하면 eth0과 lo 네트워크를 가지고 있는 것을 확인할 수 있습니다.

도커는 컨테이너에 내부 IP를 순차적으로 할당하고, 이 IP는 컨테이너를 재시작 할 때마다 변경됩니다.

이 IP들은 내부 망에서만 쓸 수 있기에 외부와 연결을 허용할 수 있게끔 바꿔주는 방법이 필요합니다.

각 컨테이너는 시작될 때 도커 엔진이 `veth`라는 이름의 네트워크 인터페이스를 생성하고 이를 통해 외부와 통신할 수 있게 도와줍니다.

도커가 설치된 호스트에서 `ipconfig`이나 `ifconfig`으로 네트워크들을 확인할 수 있습니다.

그러면 `veth`로 시작하는 인터페이스들이 있는데 이것들은 각 컨테이너의 `eth()`와 연결되어있습니다.

veth 인터페이스 뿐만 아니라 `docker0`라는 브리지도 존재하는데 각 `veth` 인터페이스와 바인딩 되어 호스트의 `eth0`인터페이스와 이어주는 역할을 합니다. 

그렇기에 다음과 같은 구조를 가집니다.

```text
의존성
      
도커 컨테이너내부[eth0, lo] -> 도커 컨테이너 외부, 호스트의[veth0] -> 호스트의[docker0] -> 호스트의[eth0]
```

# 📖 도커 네트워크 기능

docker0 브리지를 통해 외부와 통신할 수 있지만, 사용자의 선택에 따라 여러 네트워크 드라이브를 쓸 수 있다고 합니다.

- 브리지(bridge)
- 호스트(host)
- 논(none)
- 컨테이너(container)
- 오버레이(overlay)

가 있습니다. 

```docker network ls`를 통해 도커에서 기본적으로 쓸 수 있는 네트워크를 확인할 수 있습니다.

```shell
# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
31a51219ee6e   bridge    bridge    local
9c37ea44edae   host      host      local
5db3d1df48b5   none      null      local
```

브리지 네트워크는 컨테이너를 생성할때 자동으로 연결되는 `docker0` 브리지를 활용하도록 설정되어 있습니다.

이 네트워크는 172.17.0.x IP대역을 순차적으로 할당합니다.

```shell
docker network inspect bridge
```

위 명령어를 통해 네트워크에 대한 정보를 확인할 수 있습니다.

그 결과 서브넷은 172.17.0.0/16, 게이트웨이는 172.17.0.1로 설정되어 있는것을 확인할 수 있습니다.

![](/assets/img/DockerPost/start_docker_quv/C2/networkBridge.png)

그리고 밑으로 내리다보면 `Containers`항목에서 해당 네트워크를 사용중인 컨테이너 목록을 확인할 수 있습니다.

## 브리지 네트워크

`docker0`의 브리지가 아닌 사용자 정의 브리지를 새로 생성해 컨테이너에 연결하는 네트워크 구조입니다.

컨테이너는 연결된 브리지를 통해 외부와 통신할 수 있습니다.

다음 명령어를 통해 새로운 브리지 네트워크를 생성할 수 있습니다.


```shell
# docker network create --driver bridge kmsbridge
537abe5b09fa282ab9becdb7c8140916be1ebe33906622e14a2b4e01ad4f5938
```
그 다음 `docker run` 또는 `create` 명령어에 `--net` 옵션의 값을 설정하여 컨테이너가 해당 네트워크를 사용할 수 있게 설정할 수 있습니다.

```shell
docker run -i -t --name network_bridge_container \
--net kmsbridge \
ubuntu:14.04

# 접속후
ifconfig
```

![](/assets/img/DockerPost/start_docker_quv/C2/networkBridgeContainers.png)

172.18대역의 내부 IP가 할당되었습니다.

이렇게 생성된 사용자 정의 네트워크는 유동적으로 붙였다, 떼었다 할 수 있다고 합니다.

```shell
docker network disconnect kmsbridge network_bridge_container
docker network connect kmsbridge network_bridge_container
```

> 논 네트워크, 호스트 네트워크 등과 같은 네트워크 모드에는 이렇게 붙였다 떼는 명령어를 쓸 수 없습니다.
> 브리지 네트워크 또는 오버레이 네트워크와 같이 특정 IP 대역을 갖는 네트워크 모드에만 사용할 수 있습니다.
{: .prompt-tip}

만약 자동으로 설정된 네트워크가 아닌, 여러 정보를 임의로 설정하고 싶다면 다음과 같은 옵션을 설정할 수 있습니다.

```shell
docker network create --driver=bridge \
--subnet=172.72.0.0/16 \
--ip-range=172.72.0.0/24 \
--gateway=172.72.0.1 \
my_custom_network
```

## 호스트 네트워크

호스트 네트워크로 설정하면 말 그대로 호스트의 네트워크를 그대로 쓸 수 있습니다.

별도의 생성 없이 `host`라는 이름의 네트워크를 사용합니다.

```shell
docker run -i -t --name network_host \
--net host \
ubuntu:14.04

# 접속 후
ifconfig
```

접속 후 확인하면 호스트의 네트워크 환경과 같다는 것을 확인할 수 있습니다.

별도의 포트 포워드 없이 외부에서 바로 접근이 가능합니다. 

예를 들어 호스트 모드를 쓰는 컨테이너에 tomcat를 구동한다면 호스트의 `localhost:8080`으로 접근이 가능합니다.

## 논 네트워크

말 그대로 아무런 네트워크를 사용하지 않습니다.
```shell
docker run -i -t --name none_network_container \
--net none \
ubuntu:14.04

# 접속 후
ifconfig
```

네트워크를 확인해보면 로컬호스트를 나타내는 `lo` 외에는 존재하지 않는 것을 확인할 수 있습니다.

## 컨테이너 네트워크

`--net`옵션으로 container를 입력하면 다른 컨테이너의 네트워크 환경을 공유할 수 있습니다.

대표적으로 내부IP, 네트워크 인터페이스의 맥(MAC) 주소 등입니다.

```shell
docker run -i -t -d --name network_container_1 ubuntu:14.04
```

```shell
docker run -i -t -d --name network_container_2 \
--net container:network_container_1 \
ubuntu:14.04
```

이렇게 `network_container_2`는 `network_container_1`와 같은 네트워크를 공유하게 됩니다.

내부 IP를 할당받지 않으며 호스트에 `veth`로 시작하는 가상 네트워크 인터페이스도 생성되지 않습니다.

다음 명령어를 통해 확인할 수 있습니다.

```shell
docker exec network_container_1 ifconfig
...
docker exec network_container_2 ifconfig
```

![](/assets/img/DockerPost/start_docker_quv/C2/networkContainers.png)

## 브리지 네트워크와 --net-alias

브리지 타입의 네트워크와 run 명령어의 `--net-alias` 옵션을 함께 쓰면 특정 호스트 이름으로 컨테이너 여러 개에 접근이 가능하다고 합니다.

무슨 말인지 알기 위해서 `kmsbridge` 네트워크를 사용한 컨테이너 3개를 만들어봅니다.

```shell
# 1
docker run -i -t -d --name network_alias_container1 \
--net kmsbridge \
--net-alias kms727 ubuntu:14.04

# 2
docker run -i -t -d --name network_alias_container2 \
--net kmsbridge \
--net-alias kms727 ubuntu:14.04

# 3
docker run -i -t -d --name network_alias_container3 \
--net kmsbridge \
--net-alias kms727 ubuntu:14.04

# inspect 명령어로 각 컨테이너의 IP를 확인
docker inspect network_alias_container1 | grep IPAddress
>
>            "SecondaryIPAddresses": null,
            "IPAddress": "",
            "IPAddress": "172.18.0.2",
```

첫 번째 container의 주소가 172.18.0.2이므로 나머지 두 개 컨테이너의 주소는 172.18.0.3, 172.18.0.4여야 합니다.

세 개의 컨테이너에 접근할 컨테이너를 생성한 뒤 `kms727`이라는 호스트 이름으로 `ping`요청을 전송해 보겠습니다.

```shell
docker run -i -t --name network_alias_ping \
--net kmsbridge \
ubuntu:14.04
```

![](/assets/img/DockerPost/start_docker_quv/C2/networkPing.png)

컨테이너 3개의 IP로 ping이 전송되었습니다.

라운드 로빈 방식으로 보내는 IP를 결정합니다.

도커 엔진에 내장된 DNS가 kms727이라는 호스트 이름을 `--net-alias`에 의해 kms727로 설정한 컨테이너로 변환하기 때문입니다.

컨테이너 IP가 변경이 되어도 도커 내장 DNS는 이를 찾아 관리합니다.

도커는 사용자가 정의한 네트워크를 관리하는 127.0.0.11의 ip를 갖는 내장 DNS서버를 가지고, 컨테이너의 ID는 DNS 서버에 kms727(위의예제)로 등록됩니다.

그리고 kmsbridge(위의 예제)에 속한 컨테이너에 kms727에 접근하면 도커 DNS서버는 라운드 로빈을 통하여 컨테이너의 IP리스트를 반환합니다.

