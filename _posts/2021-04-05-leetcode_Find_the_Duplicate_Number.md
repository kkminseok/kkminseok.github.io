---
title: leetcode(리트코드)287-Find the Duplicate Number
author: 강민석
date: 2021-04-05 12:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 287 - Find the Duplicate Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-the-duplicate-number/>  

![](/assets/img/sample/leetcode/287/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/287/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 배열이 들어옵니다. 반복되는 요소가 있는데, 그 요소를 찾아 값을 리턴합니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        for(size_t i = 0 ; i<nums.size();++i){
            if(nums[i]==nums[i+1]) return nums[i];
        }
    return 0;
    }
};
```


-----

## 6. 결과 및 후기, 개선점


**c++ 73%**


![](/assets/img/sample/leetcode/287/result.JPG)  





