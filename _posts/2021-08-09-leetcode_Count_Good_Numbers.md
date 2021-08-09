---
title: leetcode(리트코드)-1922 Count Good Numbers(PYTHON)
author: 강민석
date: 2021-08-08 04:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1922 - Count Good Numbers  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-good-numbers/> 

![](/assets/img/sample/leetcode/1922/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1922/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- n이 들어옵니다. 
- n의 길이를 가지는 문자열이 있다고 가정합니다.(요소는 각각 0~9의 숫자를 갖는다.)
- n의 짝수번째 인덱스에는 짝수가 와야하고, 홀수번째 인덱스에는 홀수 + 소수인 값이 와야합니다.
- 만들어질 수 있는 경우의 수를 10**9 +7 로 나눈 값을 리턴하세요.




-----  

## 5. code  

코드설명


- 짝수번째 인덱스의 경우의 수는 5, 홀수번째 인덱스의 경우의 수는 4입니다. 5 * 4 * 5 * 4 * 5 ... 를 반복하면 됩니다.
- pow()와 x**y중 후자를 선택하였는데, 시간초과가 나옵니다.
- 전자도 시간초과가 나오는데, pow(x,y,mod)를 이용하면 좀 더 효율적으로 계산이 가능합니다.


**python**

```python
class Solution:
    def countGoodNumbers(self, n: int) -> int:
        mod = 10 ** 9 + 7
        return (pow(20 , (n//2),mod) * pow(5 , (n%2),mod)) % mod
        

```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1922/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


