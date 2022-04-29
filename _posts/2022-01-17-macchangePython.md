---
title: Mac OS X1(M1 Chip) Python 기본 버전 바꾸기
author: 강민석
date: 2022-01-17 00:50:50 +0800
categories: [Error]
tags: []
math: true
mermaid: true
image: 
comments: true
---

# Mac에 기본으로 등록되어 있는 Python 버전 바꾸기 삽질.

파이썬으로 바이낸스API를 이용하기 위해 알아보니, 맥에는 기본적으로 파이썬이 깔려있다는 것.

심지어 2.7버전이고 권장하지 않는다고 터미널에서 알려준다.

![](/assets/img/sample/tip/20220117/python_recary.png)  

python 버전을 바꾸기 위해서 여러 블로그에는 이렇게 나와있다.

1. 설치된 위치 확인. 
```bash
ls -l /usr/local/bin/python*
```
- 여기서 부터 에러가 난다. 왜냐하면 /usr/local이 없기 때문이다.
- 실제로 ls -l /usr/bin/python* 으로 하면 보인다. 즉, local이 빠져있다.
![](/assets/img/sample/tip/20220117/python_road.png)  

2. 파이썬 버전 변경을 위해 다음 명령어를 친다.
```bash
ln -s -f /usr/local/bin/python3.9 /usr/local/bin/python
```

여기서 나는 local 경로가 없기에 다음과 같이 쳐주었다.

```bash
sudo ln -s -f /usr/bin/python3.9 /usr/bin/python
```

그런데..

```bash
ln: /usr/bin/python: Operation not permitted
```

라는 에러가 떴다.

머릿속 : 'sudo로 썼는데 왜 접근에러가 뜬거지?'

구글링을 하다보니 mac의 '보안 및 개인정보 보호'를 수정해줘야한다해서 바꿔봤다.

![](/assets/img/sample/tip/20220117/setting.png)

그래도 해결이 되지 않았다..

뭔가 여기에 힌트가 나와있는것 같아서 고치려고 했지만 잘 되지 않았다.

<https://stackoverflow.com/questions/36730549/cannot-create-a-symlink-inside-of-usr-bin-even-as-sudo/36734569>


하는 수 없이 homebrew로 설치하기로 했다..

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

설치하고 다음 블로그의 대로 파이썬을 설치

<https://imksh.com/90>

![](/assets/img/sample/tip/20220117/python_result.png)

성공!

