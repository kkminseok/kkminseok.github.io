---
title: Programmers_Summer/Winter Coding(~2018) 스티커 모으기2(python)
author: 강민석
date: 2021-06-06 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -스티커 모으기2 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12971>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Summer/Winter Coding(~2018) 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 똑같은 문제가 프로그래머스에 있습니다. DP04 연습문제입니다.
- DP04 연습문제는 Level이 4인데 왜 이건 3인지..?

<https://kkminseok.github.io/posts/Programmers_DP04/>




-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(sticker):
    size = len(sticker)
    if size ==1 :
        return sticker[0]
    #처음 스티커를 떼어냄.
    first = [0] * size
    #두 번째 스티커를 떼어냄.
    second = [0] * size
    
    first[1] = first[0] = sticker[0]
    
    second[0] = 0
    second[1] = sticker[1]
    for i in range(2,size) :
        if i != size-1 : 
            first[i] = max(first[i-2] + sticker[i], first[i-1])
        second[i] = max(second[i-2]+sticker[i],second[i-1])

    return max(first[size-2], second[size-1])
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
