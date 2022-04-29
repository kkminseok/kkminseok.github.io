---
title: leetcode(리트코드)560-Subarray Sum Equals K
author: 강민석
date: 2021-04-22 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 560 - Subarray Sum Equals K 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/subarray-sum-equals-k/> 

![](/assets/img/sample/leetcode/560/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/560/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- nums라는 벡터가 들어옵니다.
- 연속된 부분배열을 이용해 k를 만들 수 있는 경우의 수를 구해 리턴하세요.



-----  

## 5. code  

코드설명
- 맵에 연속된 배열의 합을 저장해놓는 것이 중요합니다.


**c++**

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> um;
        um[0] ++;
        int res = 0 ;
        int sum = 0;
        for(int i = 0 ; i < nums.size();++i){
            sum += nums[i];
            res += um[sum-k];
            um[sum] ++;
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 44% 84ms**

![](/assets/img/sample/leetcode/560/result.JPG)  



