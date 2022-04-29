---
title: leetcode(리트코드)6월06일 challenge128-Longest Consecutive Sequience(python)
date: 2021-06-06 00:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 06일 - Longest Consecutive Sequience 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-consecutive-sequence/>  

![](/assets/img/sample/leetcode/128/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/128/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 06일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 부분 문자열을 합쳐서 가장 긴 연속된(1,2,3,4 ... 이런 식) 문자열의 길이를 리턴하세요.




-----  

## 5. code


**python**

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        nums.sort()
        res = 0 
        count = 1
        print(nums)
        for i in range(len(nums)-1):
            if nums[i] == nums[i+1]:
                continue
            if nums[i]+1 == nums[i+1] : 
                count+=1 
            else :
                res = max(res,count)
                count=1
        return max(res,count)
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/128/result.JPG)  

필요시 c++로 풀어드립니다.



