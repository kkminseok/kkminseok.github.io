---
title: Docker - mac M1칩 컨테이너 띄우는법
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-07-13 01:00:00 +0800
categories: [Docker,CS]
tags: [Docker]
math: true
mermaid: true
image: 
  path: /assets/img/DockerPost/docker.png
comments : true
---

운영체제가 달라서 여러가지로 애먹던 도중

```shell
docker run --platform==linux/arm64
```

이런 식으로 매 번 써주고 있었다.

스택 오버 플로우를 보니 환경변수로 설정해서 쓸 수 있더라.

```shell
export DOCKER_DEFAULT_PLATFORM=linux/amd64
```

이후에는

``shell
docker run 
```

만 해줘도 잘 된다.

