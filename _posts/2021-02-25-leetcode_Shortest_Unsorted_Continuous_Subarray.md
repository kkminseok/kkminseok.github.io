---
title: leetcode(리트코드)2월25일 challenge581-Shortest Unsorted Continuous Subarray
author: 강민석
date: 2021-02-25 16:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February 581 - Shortest Unsorted Continuous Subarray 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/shortest-unsorted-continuous-subarray/>  

![](/assets/img/sample/leetcode/581/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/581/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월25일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 해당 배열을 오름차순으로 바꿀 때 어느 부분을 바꿔야하는지 알고 그 길이를 리턴합니다.

-----  

## 5. code

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        //정렬을 미리해서 비교를합니다.
        vector<int> temp = nums;
        sort(temp.begin(),temp.end());
        int left = 0;
        int right= nums.size()-1;
        //left먼저 돕니다.
        while(left<nums.size() && nums[left]==temp[left])
            ++left;
        //만약 끝까지 돌았으면 이미 정렬된 상태입니다.
        if(left==nums.size())
            return 0;
        while(right>=0 &&nums[right]==temp[right])
            --right;
        return right-left+1;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/581/result.JPG)  

