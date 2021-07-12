---
title: leetcode(리트코드)-1929 Concatenation of Array(python)
author: 강민석
date: 2021-07-12 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1929 - Concatenation of Array  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/concatenation-of-array/> 

![](/assets/img/sample/leetcode/1929/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1929/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums라는 배열이 들어옵니다. nums를 두 번 반복한 배열을 리턴합니다.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def getConcatenation(self, nums: List[int]) -> List[int]:
        return nums + nums
        
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1929/result.JPG)  

필요시 c++로 짜드리겠습니다.



