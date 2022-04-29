---
title: Programmers_연습문제 줄 서는 방법(python)
author: 강민석
date: 2021-06-04 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -줄 서는 방법 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12936#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- brute하게 모든 경우의 수를 구하는 방법(python의 combinations)은 시간초과가 뜹니다.
- 고등학교 때 배운 경우의 수를 생각하면 먼저 한자리를 정해놓고 [정한수, ?, ?, ? ] 3가지를 세우는 방법은 3!입니다.
- 전체 경우의 수는 n인 4인 기준으로 3! * 3! * 3! * 3!의 경우의 수입니다. (맨 앞이 1인 경우 , 2인 경우, 3인 경우, 4인 경우 4가지 해서)
- 그렇기에 n!의 경우의 수가 나오고, 문제에서도 최대 20!까지 나온다고 써있습니다.
- 각 자리수는 (k-1)(찾는 수) // 남은 자릿수의 팩토리얼로 나타낼 수 있습니다.
- k-1인 이유는 0부터 찾는게 아닌, 1부터 찾는 문제이기 때문에 맨 끝의 경계선을 고려했기 때문입니다.
- k -= (나눈몫 * 팩토리얼값)으로 나타납니다.
- n이 4 이고, k = 24인 경우 [4,3,2,1]로 나타내야하는데, 위에서 4를 골라주고 k -=(몫 * 팩토리얼값)[18]은 6이 됩니다.
- 6은 n이 3이고, k = 6인 경우 [3,2,1]을 나타내는 수와 동일하므로 이런 식으로 경우의 수를 줄여가는 것입니다.
- del을 해주는 이유는 배열을 비워놔야 위에서 구한 인덱스들을 잘 찾아가기 때문입니다.







-----  

## 4. 접근 방법을 적용한 코드

```python
def makefact(num) : 
    res =1 
    for i in range(1,num+1) : 
        res*=i
    return res
def solution(n, k):
    answer = []
    person = [0] * n
    for i in range(n) : 
        person[i] = i+1 
    for i in range(n) : 
        fact = makefact(n-i-1)
        answer.append(person[(k-1)//fact])
        mod = (k-1)//fact
        del person[mod]
        k -=(mod* fact)
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
