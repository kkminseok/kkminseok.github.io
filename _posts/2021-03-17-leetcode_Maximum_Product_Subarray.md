---
title: leetcode(리트코드)152-Maximum Product Subarray
author: 강민석
date: 2021-03-17 04:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 152 - Maximum Product Subarray 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-product-subarray/>  

![](/assets/img/sample/leetcode/152/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/152/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU>에서 추천한 문제입니다.  


-----  

## 4. 문제 해석

- 곱한 값이 가장 큰 부분배열을 찾아야합니다.
- '-'을 잘 처리해주어야합니다. 1시간정도 고민하다가 discuss로 힌트를 얻었습니다.. 

-----  

## 5. code


**python**

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        maxP = -11
        minP = 11
        result = -98765321
        for i in range(len(nums)):
            if nums[i] < 0 : 
                temp = maxP
                maxP = minP
                minP = temp
            maxP = max(nums[i],maxP * nums[i])
            minP = min(nums[i],minP * nums[i])
            result = max(maxP,result)
            
        return result
```

-----  

**c++**

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if(nums.size()==0)
            return 0;
        int maxP = 1;
        int minP = 1;
        int result = INT_MIN;
        for(auto i =0 ; i<nums.size();++i)
        {
            if(nums[i]<0)
                swap(maxP,minP);
            maxP = max(nums[i],nums[i] * maxP);
            minP = min(nums[i],nums[i] * minP);
            result = max(result,maxP);
        }

    
        return result;
    }

};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

Discuss를 봤으니 x




