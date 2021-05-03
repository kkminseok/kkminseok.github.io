---
title: leetcode(리트코드)5월3일 challenge1480-Running Sum of 1d Array
date: 2021-05-03 10:17:56 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 3일 - Running Sum of 1d Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/running-sum-of-1d-array/>  

![](/assets/img/sample/leetcode/1480/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1480/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
5월 3일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- nums라는 벡터가 들어옵니다.
- 이전값들을 누적으로 더한 벡터를 리턴합니다.





-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        vector<int> DP(nums.size(),0);
        DP[0] = nums[0];
        for(int i = 1 ; i < nums.size();++i)
            DP[i] =DP[i-1] + nums[i];
        return DP;
    }
};
```



-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1480/result.JPG)  




