---
title: jekyll Blog giscus 추가하기 repo-id가 뭐야?
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-03 10:00:00 +0800
categories: [Blogging,Tutorial]
tags: [start git blog]
math: true
mermaid: true
image: 
    path: '/assets/img/blogviewPost/commentTest.png'
comments : true
---

> <https://giscus.app/ko/>이를 참고하여 작성한 글입니다.
{: .prompt-tip }

기존에 `discus`를 썼는데, `giscus`가 더 깔끔하고 보기 좋다고 생각하였다.

하지만, github계정이 있어야 한다는 점은 오히려 접근성이 더 떨어질 수 있다고도 생각하였지만..

개발자라면?..깃허브 다 있잖아?!

그래서 넣기로 하였다.

위의 사이트에서 설정값을 다음과 같이 주었다.

- Discussion 제목이 페이지 경로를 연결하기
- 카테고리는 `Announcements`
- `이 카테고리에서만 discussion 찾기` 체크
- 테마는 취향

로 체크하고 스크롤을 내리면

`giscus 활성화`가 있다.

```js
<script src="https://giscus.app/client.js"
        data-repo="[ENTER REPO HERE]"
        data-repo-id="[ENTER REPO ID HERE]"
        data-category="[ENTER CATEGORY NAME HERE]"
        data-category-id="[ENTER CATEGORY ID HERE]"
        data-mapping="pathname"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="light"
        data-lang="ko"
        crossorigin="anonymous"
        async>
</script>
```

라고 되어있는데, 위의 항목중에서 저장소의 이름을 알맞게 넣어주고 카테고리값도 넣어주면

`Enter ~`로 된 항목들이 해당 선택값으로 바뀌어 있을 것이다.

![](/assets/img/blogviewPost/commentconfig.png)

그리고 나같은 경우 blog의 `_config.yml`의 파일이 

```yaml
giscus:
    repo:            # <gh-username>/<repo>
    repo_id: 
    category: 
    category_id: 
    mapping:          # optional, default to 'pathname'
    input_position:   # optional, default to 'bottom'
    lang:           # optional, default to the value of `site.lang`
```


와 같이 되어 있어서

```yaml
giscus:
    repo: 'kkminseok/kkminseok.github.io'            # <gh-username>/<repo>
    repo_id: 'MDEwOllmasdmn,kzNDUwNzI2OTM='
    category: 'Announcements'
    category_id: 'DIC_kwDOFssazxkNxzckzjxc-J'
    mapping:          # optional, default to 'pathname'
    input_position:   # optional, default to 'bottom'
    lang:           # optional, default to the value of `site.lang`
```

처럼 giscus가 알려준 값으로 수정하니 잘 되었다.

![](/assets/img/blogviewPost/commentTest.png)

궁금한점은 댓글을 남겨주세요.