---
title: npm - self signed certificate in certificin Error 해결법
author: 강민석
date: 2022-04-04 01:34:40 +0800
categories: [Error,NPM]
tags: [error]
math: true
mermaid: true
image: 
comments: true
---


npm install 시에 발생하는 

'Error:SSL Error: SELFT_SIGNED_CERT_IN_CHAIN' error 또는

'self signed certificate in certificin Error' 해결.

인증서문제 같은데

```bash
npm config set strict-ssl false
```

로 해결.

인증서 검사를 안하겠다는 것. 


쓸만한 글 : <http://blog.securekim.com/2019/03/bash-gradle-python-wget-nodejsnpm-apt.html>














 
