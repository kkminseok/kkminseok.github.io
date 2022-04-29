---
title: leetcode(리트코드)-838 Push Dominoes(PYTHON)
author: 강민석
date: 2021-07-21 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 838 - Push Dominoes  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/push-dominoes/> 

![](/assets/img/sample/leetcode/838/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/838/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- dominoes가 들어온다.
- dominoes의 R은 오른쪽으로 도미노이고 L은 왼쪽으로 도미노한다..
- 예제처럼 2개가 쌓여있다해서 힘이 더 쌔고 그런건 없는 것같다.


-----  

## 5. code  

코드설명

- '.'을 기준으로 왼쪽와 오른쪽을 탐색해서 "LL,LR,RL,RR"인 경우마다 처리를 다르게 해주어 해결했다.

**python**

```python
class Solution:
    def pushDominoes(self, dominoes: str) -> str:
        idx = 0
        dominoes = list(dominoes)
        while idx < len(dominoes) :
            if dominoes[idx] == '.' :
                left = idx
                right = idx
                while left > 0 and dominoes[left] == '.' : 
                    left-=1
                while right+1 < len(dominoes) and dominoes[right] == '.' : 
                    right+=1
                if dominoes[right] == 'L' :
                    if dominoes[left] == 'R':
                        while left+1 < right-1 : 
                            dominoes[left+1] = 'R'
                            dominoes[right-1] = 'L'
                            left+=1
                            right-=1
                    else : 
                        while left < right :  
                            dominoes[left] = 'L'
                            left+=1
                else : 
                    if dominoes[left] == 'R' :
                        while left <= right : 
                            dominoes[left] = 'R'
                            left+=1 
            idx+=1
        
        return ''.join(dominoes)
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/838/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


