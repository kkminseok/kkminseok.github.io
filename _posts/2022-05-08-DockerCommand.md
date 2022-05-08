---
title: Docker(2) - Docker Command 기초
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-08 01:00:00 +0800
categories: [Docker,CS]
tags: [Docker]
math: true
mermaid: true
image: 
  path: /assets/img/sample/logo.jpg
comments : true
---

# 1. Serach

```bash
docker search redis ...
```

> `docker serach 이미지명`은 `Docker Hub`에 등록된 이미지들을 찾는 역할을 합니다.
{: .prompt-tip }

![](/assets/img/DockerPost/pyrasis_docker/search.png)


# 2. Pull

```bash
docker pull ubuntu:latest ...
```

> `docker pull 이미지명:태그명`은 `Docker Hub`에 등록된 이미지를 로컬 레포지토리로 가져옵니다.
{: .prompt-tip }

![](/assets/img/DockerPost/pyrasis_docker/pull.png)

# 3. Images

```bash
docker images
```

>`docker images`는 모든 이미지 목록을 출력합니다. `docker images ubuntu`라고 하면 ubuntu에서 태그만 다른 값들을 출력합니다.
{: .prompt-tip }

![](/assets/img/DockerPost/pyrasis_docker/images.png)

# 4. Run

```bash
docker run -i -t --name kms ubuntu:latest /bin/bash
```

>`docker run <옵션명> <이름명> <이미지명> <실행할 파일> `의 형식을 갖는 run 명령어는 다음과 같이 해석할 수 있습니다.
{: .prompt-tip }

- `-i`, `-t` : 선택된 `bash`셸에 입력및 출력이 가능.
- `--name` : 이름 입력이 가능. 위의 경우 `kms`로 지정. 지정하지 않으면 Docker가 알아서 지정.

![](/assets/img/DockerPost/pyrasis_docker/run.png)

```bash
exit
```  
명령어로 빠져나올 수 있습니다.

# 5. Ps

```bash
docker ps -a
```

>`docker ps <option>`은 실행되고 있는 컨테이너 또는 모든 컨테이너 목록을 출력합니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/ps.png)

# 6. Start

```bash
docker start kms
```

>`docker start <컨테이너 이름>`으로 구성된 명령어는 입력한 컨테이너를 찾아 실행합니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/start.png)

# 7. Restart

```bash
docker restart kms
```

>`docker restart <컨테이너 이름>`으로 구성된 명령어는 해당 컨테이너를 재시작합니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/restart.png)

# 8. Attach

```bash
docker attach kms
```

>`docker attach <컨테이너 이름>`으로 구성된 명령어는 해당 컨테이너에 접속이 가능해집니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/attach.png)

# 9. Exec

```bash
docker exec kms echo "hi docker im kms"
```

>`docker exec <컨테이너이름> <명령어> <매개변수>`로 이루어진 명령어는 **실행중인 컨테이너**에 사용자가 지정한 명령어를 외부에서 보내 실행하게 합니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/exec.png)

# 10. Stop

```bash
docker stop kms
```

>`docker stop <컨테이너 이름>`은 해당 컨테이너를 중지시킵니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/stop.png)

# 11. Rm

```bash
docker rm kms
```

>`docker rm <컨테이너 이름>`은 해당 컨테이너를 삭제합니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/rm.png)

# 12. Rmi

```bash
docker rmi ubuntu:latest
```

>`docker rmi <이미지명>:<태그명>`은 해당 이미지를 삭제합니다.
{: .prompt-tip}

![](/assets/img/DockerPost/pyrasis_docker/rmi.png)


