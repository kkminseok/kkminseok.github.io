---
title: leetcode(리트코드)3월18일 challenge376-Wiggle Subsequence
author: 강민석
date: 2021-03-19 00:00:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 18일 - Wiggle Subsequence 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/wiggle-subsequence/>  

![](/assets/img/sample/leetcode/376/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/376/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 18일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- wiggle이란 인접한 배열의 차이가 + - 가 반복되는 배열을 말합니다.
- [1, 7, 4, 9, 2, 5]의 각 인접한 배열의 차이는 [6,-3,5,-7,3]으로 이렇게 인접한 배열의 차이가 + - 가 반복되는 wiggle 부분배열의 최대길이를 반환합니다.
- DP로 풀었습니다.


-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()==1)
            return 1;
        int* DP = new int [nums.size()];
        DP[0]=1;
        bool minusC = true;
        bool plusC = true;
        
        for(size_t i =1;i<nums.size();++i)
        {
            if(minusC && nums[i-1] < nums[i])
            {
                minusC= false;
                plusC= true;
                DP[i] = DP[i-1]+1;
            }
            else if(plusC && nums[i-1]>nums[i])
            {
                plusC = false;
                minusC = true;
                DP[i] = DP[i-1] + 1;
            }
            else
                DP[i] = DP[i-1];
        }
        
        return DP[nums.size()-1];
    }
};
```

----- 


**python**

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        if len(nums) == 1: 
            return 1
        DP = [0] * len(nums)
        DP[0]= 1
        plusC = 1
        minusC = 1
        
        for i in range(1,len(nums)):
            if minusC==1 and nums[i] < nums[i-1] : 
                minusC = 0
                plusC = 1
                DP[i] = DP[i-1] + 1
            elif plusC==1 and nums[i] > nums[i-1] : 
                minusC = 1
                plusC = 0
                DP[i] = DP[i-1] + 1
            else :
                DP[i] =DP[i-1]
         
        return DP[len(nums)-1]
```

-----

## 6. 결과 및 후기, 개선점

**c++ 100% python 62%**


![](/assets/img/sample/leetcode/376/result.JPG)  

