---
title: leetcode(리트코드)-162 Find Peak Element(python)
author: 강민석
date: 2021-06-04 04:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 162 - Find Peak Element 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-peak-element/> 

![](/assets/img/sample/leetcode/162/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/162/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 인덱스를 기준으로 그 왼쪽값, 오른쪽값 보다 큰 인덱스를 찾아 리턴합니다.
- 양쪽 끝의 그 왼쪽, 오른쪽값은 (-무한대)라고 가정합니다.
- 무조건 중간값을 기준으로 한쪽은 결과가 나올 수 밖에 없습니다.(증명은 못하겠습니다. 반례가 나오지 않음.)



-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        def binary(left,right,nums):
            if left == right : 
                return left
            mid = (left + right) //2
            mid2 = mid+1
            if nums[mid] > nums[mid2] :
                return binary(left,mid,nums)
            else : 
                return binary(mid2,right,nums)
                
            
        return binary(0,len(nums)-1,nums)
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/162/result.JPG)  

필요시 c++로 짜드리겠습니다.



