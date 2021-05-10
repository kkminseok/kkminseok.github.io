---
title: Programmers_다음 큰 숫자
author: 강민석
date: 2021-05-06 11:11:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -다음 큰 숫자 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12911>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- c++에 bitset<>이 있듯이 파이썬에서도 bin()이라는 binary로 바꿔주는 함수가 있었습니다.
- 순차적으로 풀면 효율성이 좋지 않을거라 생각했지만, 생각 이상으로 빨라서 당황했습니다.
- bin()과 bin에서 제공하는 count()함수를 잘 이용
해야겠습니다.




-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(n):
    answer = 0
    binaryn = bin(n)
    getcount = binaryn.count('1')
    for i in range(n+1, 1000000):
        temp = bin(i)
        if getcount == temp.count('1') :
            return i
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
