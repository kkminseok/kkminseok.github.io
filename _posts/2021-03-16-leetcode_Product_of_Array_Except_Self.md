---
title: leetcode(리트코드)238-Product of Array Except Self
author: 강민석
date: 2021-03-16 01:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 238 - Product of Array Except Self 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/product-of-array-except-self/>  

![](/assets/img/sample/leetcode/238/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/238/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.   
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU>에서 추천한 문제입니다.  


-----  

## 4. 문제 해석

- 나눗셈을 사용하지 않고 인덱스의 곱들을 구합니다. 단, 선택된 인덱스를 제외한 모든 인덱스의 곱입니다.

-----  

## 5. code

**python**

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        size = len(nums)
        result = [1] * size
        for i in range(1,size):
            result[i] = result[i-1] * nums[i-1]
        right = 1
        for i in range(size-1,-1,-1):
            result[i] *= right
            right *= nums[i]
        return result
```

----- 

**c++**

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n);
        result[0] = 1;
         for(size_t i =1;i<n;++i)
         {
             result[i] = result[i-1] * nums[i-1];
         }
        int right = 1;
        for(int i = n-1; i>=0;--i)
        {
            result[i] *= right;
            right *= nums[i];
        }
        return result;   
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 93% python 72%**  
![](/assets/img/sample/leetcode/238/result.JPG)  


