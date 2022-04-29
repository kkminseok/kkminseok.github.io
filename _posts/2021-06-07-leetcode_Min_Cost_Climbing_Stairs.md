---
title: leetcode(리트코드)6월07일 challenge746-Min Cost Climbing Stairs(python)
date: 2021-06-07 00:01:56 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 07일 - Min Cost Climbing Stairs 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/min-cost-climbing-stairs/>  

![](/assets/img/sample/leetcode/746/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/746/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
6월 07일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 비용(Cost)가 주어집니다. 
- 해당 칸에 도착했을 때 비용을 지불하고 계단을 1이나 2씩 오를 수 있습니다.
- 위와 같은 방식으로 계단을 오를 때 최소환의 비용을 리턴하세요.




-----  

## 5. code


**python**

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        cost.append(0)
        res = [0] * (len(cost))
        res[0] = cost[0]
        res[1] = cost[1]
        for i in range(2,len(cost)):
            res[i] = min(res[i-2] + cost[i], res[i-1] + cost[i])
        return res[len(res)-1]
        
        
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/746/result.JPG)  

필요시 c++로 풀어드립니다.



