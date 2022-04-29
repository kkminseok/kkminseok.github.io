---
title: Programmers_숫자의 표현
author: 강민석
date: 2021-05-05 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -숫자의 표현 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12924#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 생각할 것이 좀 있습니다.
- 홀수인 경우에는 무조건 어떤수 + 어떤수로 나타낼 수 있으며, 나누어떨어지는 수를 중간으로 기준으로 일렬로 세우는 방법의 갯수를 세면 됩니다.
- 짝수인 경우에는 어떤수 + 어떤수로 나타낼 수 없습니다. 약수가 홀수인 경우에만 n으로 나타낼 수 있다는 것을 깨달아서 코드를 작성하였습니다.



-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(n):
    answer = 0
    if(n%2 == 1):
        for i in range (1,n+1):
            if(n%i ==0):
                answer+=1
    else:
        for i in range(1,n+1) : 
            if(n%i==0 and i%2==1):
                answer+=1
    return answer
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
