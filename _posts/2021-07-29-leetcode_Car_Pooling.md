---
title: leetcode(리트코드)-1094 Car Pooling(PYTHON)
author: 강민석
date: 2021-07-29 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1094 - Car Pooling  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/car-pooling/> 

![](/assets/img/sample/leetcode/1094/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1094/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- trips가 주어집니다.
- capacity가 주어집니다.
- trips[0]은 사람 수 , trips[1]은 탑승 시간, trips[2]는 내린 시간이라고 합니다.
- capacity만큼 사람들을 수용할 수 있을 때 모든 사람들을 수용할 수 있는 지 확인하여 리턴하세요.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        lifetime = [0] * 1001
        for i in range(len(trips)):
            lifetime[trips[i][1]] += trips[i][0]
            lifetime[trips[i][2]] -= trips[i][0]
        for i in range(1001):
            if capacity < 0 :
                return False
            capacity -= lifetime[i]
                    
        return True
                
                               
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1094/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


