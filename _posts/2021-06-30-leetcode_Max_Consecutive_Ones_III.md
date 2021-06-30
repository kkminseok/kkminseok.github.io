---
title: leetcode(리트코드)6월29일 challenge1004-Max Consecutive Ones III(python)
date: 2021-06-30 03:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 29일 - Max Consecutive Ones III 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/max-consecutive-ones-iii/>  

![](/assets/img/sample/leetcode/1004/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1004/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 29일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- nums가 주어집니다. k 만큼 0을 1로 바꿀 수 있을 때 연속된 1의 길이가 가장 긴 값을 리턴하세요.




-----  

## 5. code


**python**

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        i = 0 
        for j in range(len(nums)):
            k-= 1 - nums[j]
            if k < 0 :
                k+= 1-nums[i]
                i +=1
        return j-i+1
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1004/result.JPG)  

필요시 c++로 풀어드립니다.



