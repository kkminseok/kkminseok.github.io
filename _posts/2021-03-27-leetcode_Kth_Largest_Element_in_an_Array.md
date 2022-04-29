---
title: leetcode(리트코드)215-Kth Largest Element in an Array
author: 강민석
date: 2021-03-27 00:01:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 215 - Kth Largest Element in an Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/kth-largest-element-in-an-array/>  

![](/assets/img/sample/leetcode/215/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/215/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 간단한 문제입니다. k번째로 큰 수를 배열에서 찾아 리턴합니다.



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        return nums[nums.size()-k];
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 97% python ??%** 
![](/assets/img/sample/leetcode/215/result.JPG)  

