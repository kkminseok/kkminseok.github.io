---
title: leetcode(리트코드)153-Find Minimum in Roated Sorted Array
author: 강민석
date: 2021-03-18 05:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 153 - Find Minimum in Rotated Sorted Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/>  

![](/assets/img/sample/leetcode/153/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/153/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 한칸씩 뒤로 밀려버린 vector가 인풋으로 들어옵니다. 원래 벡터로 만들었을 때 가장 작은 값을 리턴합니다.
- 너무 쉽게 푼것 같습니다. 이렇게 푸는게 아닌 것 같은데..



-----  

## 5. code

**python**


```pyhton
class Solution:
    def findMin(self, nums: List[int]) -> int:
        for i in range(len(nums)-1) : 
            if nums[i] > nums[i+1] : 
                return nums[i+1]
        return nums[0]
```     

-----  

**c++**

```c++

class Solution {
public:
    int findMin(vector<int>& nums) {
        for(size_t i= 0;i<nums.size()-1;++i)
        {
            if(nums[i] >nums[i+1])
                return nums[i+1];
        }
        return nums[0];
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 50% python 71%** 
![](/assets/img/sample/leetcode/153/result.JPG)  


**binary search로 푼 code c++**


```c++
 int findMin(vector<int> &num) {
        int start=0,end=num.size()-1;
        
        while (start<end) {
            if (num[start]<num[end])
                return num[start];
            
            int mid = (start+end)/2;
            
            if (num[mid]>=num[start]) {
                start = mid+1;
            } else {
                end = mid;
            }
        }
        
        return num[start];
    }
```
