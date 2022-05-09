---
title: Docker(3) - Docker로 nginx 띄우기
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-08 02:00:00 +0800
categories: [Docker,CS]
tags: [Docker]
math: true
mermaid: true
image: 
  path: /assets/img/sample/logo.jpg
comments : true
---

안 쓰는 폴더를 만들어서 `Dockerfile`을 만들어줍시다.

```bash
#Dockerfile

FROM ubuntu:latest

RUN apt-get update
RUN apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
RUN chown -R www-data:www-data /var/lib/nginx

VOLUME ["/data", "/etc/nginx/site-enabled", "/var/log/nginx"]

WORKDIR /etc/nginx

CMD ["nginx"]

EXPOSE 80
EXPOSE 443

```

