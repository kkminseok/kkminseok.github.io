---
title: Programmers_위클리 챌린지 모음사전
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-06-29 01:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
  path: 
comments : true
---



**프로그래머스 위클리 챌린지 - 교점에 별 만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/84512#>

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
위클리 챌린지 문제 입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 규칙을 찾으면 다음 배열이 어떻게 이어지는 잘 생각해서 해결하면 됩니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(word):
    answer = 0
    dict = {
        1 : 1,
        2 : 6,
        3 : 31,
        4 : 156,
        5 : 781
    }
    word_list = ['A','E','I','O','U']
    for index in range(len(word)):
        w = word[index]
        answer+=1
        for j in range(len(word_list)):
            char = word_list[j]
            if char == w :
                answer += dict[5-index] * j
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.

