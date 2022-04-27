---
title: jekyll로 gitblog 시작하기[1] - 초기설정
author: 강민석
date: 2020-12-18 12:00:00 +0800
categories: [Blogging,Totorial]
tags: [start git blog]
math: true
mermaid: true
image: /assets/img/sample/devices-mockup.png
comments : true
---

# 1. git blog fork, clone, init

 깃 블로그를 시작하기 위해서는 먼저 테마를 골라야합니다.<br>
 
 테마사이트는 굉장히 많지만 나와같은 테마는 여기서 볼 수 있습니다.<br>

<http://jekyllthemes.org/><br>

- 원하는 theme를 고르고 fork를 합니다.

- fork를 한 뒤 clone를 해줍니다. <u>그 전 각 테마에 해당하는 readme 파일을 읽어주세요.!!!</u>


### git Terminal

```terminal
    $ git clone https://github.com/USERNAME/USERNAME.github.io.git -b master --single-branch
```

- clone을 성공적으로 한 뒤, cmd로 해당 폴더로 들어간 뒤 다음 명령어를 쳐줍니다. 만약 ruby가 설치되어있지 않다는 오류가 뜨면 ruby를 설치합니다.
```terminal
$ bundle install
```
- 초기화 작업을 해줍니다.
```terminal
$ bash tools/init.sh
```
<br>

# 2. git blog push

자신의 깃 주소에 들어가서 fork한 저장소의 이름을 바꿉니다.<br>

꼭 자신의 깃아이디와 똑같이 만들어야합니다.<br>
![](/assets/img/sample/gitrepo.jpg)<br>
똑같이 만든 뒤, 자신의 사이트에 들어가서 홈페이지가 잘 나오는지 봅니다.<br>
예시<br>
<https://kkminseok.github.io> <br>
<https://Username.github.io><br>
자신의 root 디렉토리에서 _config.yml의 파일을 찾아 다음을 수정해줍니다.
- `url` <br>
> 자신의 깃 주소를 입력합니다. ex) https://kkminseok.github.io
- `avatar`
> 자신의 로고이미지를 입력합니다. ex) root기준 path /~
- `timezone`
> 자신이 살고 있는 곳. ex) 한국에 사니까 Asia/Seoul
- `theme_mode`
> dark mode or light mode

수정이 완료되었으면, git add -> commit -> push를 해줍니다.
```terminal
$ git add .
$ git commit -m "test"
$ git push
```
<strong>!git에서 설정을 해줘야할 것이 있습니다.!</strong><br>
git Setting에 들어가서 밑으로 쭉내리다보면 GitHub Pages가 있습니다.
![](/assets/img/sample/2020_12_18_2.jpg)
Branch가 master로 되어있을 텐데 gh-pages로 바꿔줘서 Save버튼을 눌러줍니다.<br>
해당 브런치를 바꿔주지 않으면  git에서 보안관련 오류가 계속 뜹니다.
대표적인 모습은 다음과 같습니다.. 무수히 많은 에러들
![](/assets/img/sample/2020_12_18/builderror.jpg)
> github Run filed:Automatic build - master .... 참고로 추후 포스트에 오타 등이 요인이 될 수 있습니다.
<br>

뿐만아니라 추후 sitemap 제출시에도 반영이 안되는 경우가 있으므로 꼭꼭!! 바꿔줍시다.<br>
<strong>기억에 의존하여 과정이 틀릴 수 있지만, 많은 오류를 겪어본 결과 모두 readme파일을 읽지않아서 발생한 오류들 이었습니다. 꼭 readme파일을 읽고 순서대로 진행해주세요.!!</strong>

다음 포스트에는 sitemap을 제출하여 구글 서치에 노출되는 방법을 알아보겠습니다. <br>
문의 사항은 댓글에 적어주세요.

-----
저에게 도움을 주신 [**https://chanhuiseok.github.io/**](https://chanhuiseok.github.io/)님 감사합니다.
![](/assets/img/sample/2020_12_18/thk.png)

