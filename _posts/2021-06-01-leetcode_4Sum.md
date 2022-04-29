---
title: leetcode(리트코드)-18 4Sum(python)
author: 강민석
date: 2021-06-01 07:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 18 - 4Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/4sum/> 

![](/assets/img/sample/leetcode/18/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/18/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 3Sum 문제와 비슷하게 리스트에서 4개를뽑아 target을 만들어야합니다.
- 중복을 허용하지 않고, 순서바꾸는 것도 허용하지 않습니다.




-----  

## 5. code  

코드설명 필요하시면 말씀해주세요.

**python**

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        size = len(nums)
        res = []
        for i in range(size-3):
            if i==0 or (i>0 and nums[i] !=nums[i-1]):
                for j in range(i+1,size-2):
                    if j == i+1 or (j > i+1 and nums[j] != nums[j-1]) :
                        lo,hi,ts = j+1,size-1,target-(nums[i] +nums[j])
                        while lo < hi :
                            if nums[lo] + nums[hi] == ts :
                                res.append([nums[i],nums[j],nums[lo],nums[hi]])
                                while lo <hi and nums[lo] == nums[lo+1] :
                                    lo+=1
                                while lo < hi and nums[hi] == nums[hi-1] : 
                                    hi-=1
                                lo+=1
                                hi-=1
                            elif nums[lo] + nums[hi] < ts :
                                lo+=1
                            else:
                                hi-=1
        return res
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/18/result.JPG)  

필요시 c++로 짜드리겠습니다.



