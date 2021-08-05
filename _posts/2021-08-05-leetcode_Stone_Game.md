---
title: leetcode(리트코드)-877 Stone Game(PYTHON)
author: 강민석
date: 2021-08-05 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 877 - Stone Game  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/stone-game/> 

![](/assets/img/sample/leetcode/877/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/877/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- piles라는 짝수의 크기를 가진 배열이 들어옵니다.
- alex와 lee가 돌아가면서 수를 뽑을 때 합계 포인트가 가장 많은 사람이 이깁니다.
- alex가 이길 수 밖에 없으면 True 아니라면 False를 리턴하세요.

-----  

## 5. code  

코드설명

- alex가 이길 수 밖에 없다고 합니다. 또한, 너무 쉽게 풀려서 Medium의 레벨이 아닌 것 같습니다.
- 가장 큰 것부터 뽑아 계산하여 풀었습니다.


**python**

```python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        piles.sort()
        dq = deque(piles)
        alex = 0
        lee = 0
        while dq : 
            alex += dq.pop()
            lee += dq.pop()
        return True if alex > lee else False
                     
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/877/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


