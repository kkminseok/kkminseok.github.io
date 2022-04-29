---
title: leetcode(리트코드)-189 Rotate Array(python)
author: 강민석
date: 2021-05-19 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 189 - Rotate Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/rotate-array/> 

![](/assets/img/sample/leetcode/189/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/189/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top Interview 문제입니다.


-----  

## 4. 문제 해석

- k로 들어온 숫자만큼 오른쪽으로 회전(?) 각 요소들을 옮깁니다.
- discuss에서 나온 방식이 가장 먼저 떠올랐지만, 해당 구문이 python에서 어떤 우선순위를 가지고 처리할 지 몰라서 하나하나 옮기는 식으로 작성하였습니다.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        size = len(nums)
        k = k%size
        tempres = copy.deepcopy(nums)
        print(tempres)
        for i in range(size):
            rotate = (i+k) %size
            nums[rotate] = tempres[i]
    # dicuss 코드            
    def rotate2(self, nums, k):
        n = len(nums)
        k = k % n
        nums[:] = nums[n-k:] + nums[:n-k]
        
        
        
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/189/result.JPG)  

동작방식 참고



