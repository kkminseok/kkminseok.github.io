---
title: leetcode(리트코드)-1893 Check if All the Integers in a Range Are Covered(python)
author: 강민석
date: 2021-06-20 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1893 - Check if All the Integers in a Range Are Covered 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-all-the-integers-in-a-range-are-covered/> 

![](/assets/img/sample/leetcode/1893/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1893/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- ranges라는 범위가 주어집니다. left와 right가 주어질 때 해당 값들 left 2 right 5라면 (2,3,4,5)가 ranges에 포함되면 Truef를 리턴하고, 포함되지 않으면 False를 리턴하세요.

- input의 크기가 크지 않아서 brute하게 풀 수 있지만, 시간을 아끼기 위한 방법을 고려했습니다.


-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def isCovered(self, ranges: List[List[int]], left: int, right: int) -> bool:
        ranges.sort()
        num = left
        for i in range(len(ranges)) : 
            while num <=right and ranges[i][0] <= num and num <= ranges[i][1] :
                num+=1
            if num > right:
                return True
        return False
        
    
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1893/result.JPG)  

필요시 c++로 짜드리겠습니다.



