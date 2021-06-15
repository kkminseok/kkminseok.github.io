---
title: leetcode(리트코드)6월15일 challenge473-Matchsticks to Square(python)
date: 2021-06-15 03:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 15일 - Matchsticks to Square 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/matchsticks-to-square/>  

![](/assets/img/sample/leetcode/473/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/473/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 15일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 리스트로 값들이 주어집니다. 해당 값들을 이용해서 각 길이가 같은 정사각형을 만들 수 있으면 True 없으면 False를 리턴하세요.

- discuss의 도움을 받아 DFS로 풀었습니다. 값을 돌면서 matchsticks의 target의 값을 갱신해줍니다.




-----  

## 5. code


**python**

```python
class Solution:
    def makesquare(self, matchsticks: List[int]) -> bool:
        sumnum = sum(matchsticks)
        if sumnum%4 !=0:
            return False
        target = [sumnum//4] * 4
        matchsticks.sort(reverse = True)
        def bfs(num,depth,target) :
            if depth == len(num) : 
                return True
            for i in range(4):
                if target[i] >= num[depth] : 
                    target[i] -= num[depth]
                    if bfs(num,depth+1,target) : return True
                    target[i] += num[depth]
            return False
        return bfs(matchsticks,0,target)
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/473/result.JPG)  

필요시 c++로 풀어드립니다.



