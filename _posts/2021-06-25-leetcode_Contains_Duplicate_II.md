---
title: leetcode(리트코드)-219 Contains Duplicate II(python)
author: 강민석
date: 2021-06-25 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 219 - Contains Duplicate II   문제입니다.**

## 1. 문제
<https://leetcode.com/problems/contains-duplicate-ii/> 

![](/assets/img/sample/leetcode/219/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/219/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- nums배열이 들어옵니다. 같은 수인 어떤 인덱스 2개를 집었을 때 그 인덱스들의 값이 k보다 작거나 같으면 True를 리턴하고, 그런 인덱스가 없다면 False를 리턴합니다.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        dic = {}
        
        for i in range(len(nums)):
            if nums[i] in dic : 
                if abs(i - dic[nums[i]]) <=k :
                    return True
                else :
                    dic[nums[i]] = i 
            else:
                dic[nums[i]] = i
        return False
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/219/result.JPG)  

필요시 c++로 짜드리겠습니다.



