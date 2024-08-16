---
title: Programmers_3xn 타일링
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-06-28 01:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
comments : true
---

**프로그래머스 - 3 x n 타일링 문제 입니다.**


## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12902#>

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 대표적인 DP문제입니다.
- 홀수일때 채울 수 있는 경우의 수는 없습니다.
- 4일때 이후로는 새로운 모양이 2개씩 더 생기므로 그것을 고려하여 알고리즘을 작성해야합니다.
- 일반적인 경우는 [n-2] * 3을 통해 3가지 경우의 수를 곱해주면 되지만, 새로운 문양은 무조건 2가지가 생기고(d[0] * 2) 새로운 문양에 대해서 앞 뒤로(*2)붙일 수 있는 경우의 수가 생기므로 점화식은 다음과 같다.

> dp[n] = dp[n-2] * 3 + dp[0] * 2(새로운 모양) + dp[2] * 2(dp[n-2]의 새로운 모양에 대해서 dp[2]가앞 뒤로 붙음 ) .... dp[n-4]까지


-----  

## 4. 접근 방법을 적용한 코드

```python
from collections import deque
def solution(n):
    dp = deque([0] * (n+1))
    dp[0] = 1
    if n >= 2:
        dp[2] = 3
    for i in range(4,n+1, 2):
        dp[i] = dp[i-2] * 3
        for j in range(0,i-3,2):
            dp[i] += dp[j] * 2
    return dp[n] % 1000000007
```


-----



## 5. 결과

![](/assets/img/sample/Programmers/All/3xnTile.png)

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.

