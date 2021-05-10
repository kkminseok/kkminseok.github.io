---
title: Programmers_예상 대진표
author: 강민석
date: 2021-05-10 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -예상 대진표 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12985>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2017 팁스타운 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제는 직관적으로 어렵지 않습니다.
- A가 B보다 클 수 있다는 사실을 간과하여 시간이 좀 걸렸습니다.





-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(n,a,b):
    answer = 1
    if a> b:
        a,b = b,a
    while 1 : 
        print(a,b)
        if a%2 ==1 and b%2 ==0 and a+1 == b :
            return answer
        a = (a+1)//2
        b = (b+1)//2
        answer+=1
        

    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
