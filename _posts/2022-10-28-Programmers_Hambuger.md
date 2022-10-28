---
title: Programmers_햄버거 만들기(python)
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-10-28 00:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
  path: 
comments : true
---

> 이 글을 보시기 전에 문제를 풀기 위해 충분한 생각을 하셨나요? 답을 안 보고 푸는게 최대한 고민하는게 가장 중요하다고 생각합니다.!!
{: .prompt-warning}


**프로그래머스 햄버거 만들기 문제 입니다.**

## 1.☑️ 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/133502?language=python3>

-----  

## 2.☑️ 분류 및 난이도

Programmers 문제입니다.  

Level 1난이도의 문제입니다. 


-----  

## 3.☑️ 생각한 것들(문제 접근 방법)

- 나오는 순서가 일정해야하고 그 순서가 문제에서 주어진 조건과 동일하다면 없애야합니다 -> 스택 생각.


-----  

## 4.☑️ 접근 방법을 적용한 코드

```python
from collections import deque
def solution(ingredients):
    answer = 0
    stack = deque()
    
    for ingredient in ingredients:
        stack.append(ingredient)
        if ingredient == 1 and len(stack) > 3 and stack[-2] == 3 and stack[-3] == 2 and stack[-4] == 1:
            for i in range(4):
                stack.pop()
            answer+=1
    
    return answer
```


-----

구간을 탐색하고 맞다면 pop 해주고 아니면 넘어갑니다.



## 5.☑️ 결과

> c++로 작성이 필요하거나 도움이 필요하시면 댓글을 작성해주세요.!! 기록용이라 설명이 자세하지 않습니다.
{: .prompt-info }