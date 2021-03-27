---
title: leetcode(리트코드)377-Combination Sum IV
author: 강민석
date: 2021-03-26 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 377 - Combination Sum IV 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/combination-sum-iv/>  

![](/assets/img/sample/leetcode/377/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/377/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- DP문제입니다.
- 주어진 nums를 가지고 target을 만들 수 있는 경우의 수를 리턴합니다.
- 점화식은 DP[i] += DP[i-nums[i]]인데, 1부터 시작해서 nums를 조사해, 만들 수 있는 경우의 수를 전부 더해주는 식입니다.



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        unsigned int* DP = new unsigned int[sizeof(int) * (target+1)];
        memset(DP,false,sizeof(int) * (target+1));
        DP[0]= 1;
        for(int i =1;i<=target; ++i)
        {
            for(int j =0;j<nums.size();++j)
            {
                if(i >= nums[j])
                {
                    DP[i] += DP[i-nums[j]];
                }
            }
        }
        
        return DP[target];
    }
};


/*
        int combinationSum4(vector<int>& nums, int target) {
        vector<unsigned int> result(target + 1);
        result[0] = 1;
        for (int i = 1; i <= target; ++i) {
            for (int x : nums) {
                if (i >= x) result[i] += result[i - x];
            }
        }
        
        return result[target];
    }
*/


```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/377/result.JPG)  






