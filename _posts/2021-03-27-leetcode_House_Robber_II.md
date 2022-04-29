---
title: leetcode(리트코드)213-House Robber II
author: 강민석
date: 2021-03-27 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 213 - House Robber II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/house-robber-ii/>  

![](/assets/img/sample/leetcode/213/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/213/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- DP문제입니다.
- 프로그래머스에 똑같은 문제가 있습니다. '도둑질'



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1)
            return nums[0];
        //DP를 2개만듦
        //DP는 첫 번째 집을 들르는 경우
        //DP2는 두 번째 집을 들르는 경우
        int* DP = new int[nums.size()];
        int* DP2 = new int[nums.size()];
        DP[0] = nums[0];
        DP[1] = nums[0];
        
        DP2[0] = 0;
        DP2[1]=nums[1];
        
        for(size_t i =2;i<nums.size();++i)
        {
            if(i !=nums.size()-1)
            {
                DP[i] = max(DP[i-2] + nums[i], DP[i-1]);
            }
            DP2[i] = max(DP2[i-2]+nums[i],DP2[i-1]);
        }
        int result = max(DP[nums.size()-2],DP2[nums.size()-1]);
        
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/213/result.JPG)  






