---
title: leetcode(리트코드)198-House Robber
author: 강민석
date: 2021-03-22 05:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 198 - House Robber 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/house-robber/>  

![](/assets/img/sample/leetcode/198/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/198/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 인접한 배열을 접근하지 않고 배열 안에 있는 값들을 더할 때 최대값을 구하세요.
- 점화식은 DP[i] = max(DP[i-2],DP[i-3]) + nums[i]로 바로 접근할 때와 하나 건너뛰고 접근하는 경우의 수가 있습니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1)
            return nums[0];
        if(nums.size()==2)
            return max(nums[0],nums[1]);
        int* DP = new int[nums.size()];
        DP[0] = nums[0];
        DP[1] = nums[1];
        DP[2] = nums[2] + DP[0];
        int result = max(DP[2],DP[1]);
        for(size_t i = 3; i<nums.size();++i)
        {
            DP[i] = max(DP[i-2],DP[i-3]) + nums[i];
            result = max(result,DP[i]);
        }
        
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/198/result.JPG)  






