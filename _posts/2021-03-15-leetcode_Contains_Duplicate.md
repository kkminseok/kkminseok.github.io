---
title: leetcode(리트코드)217-Contains Duplicate
author: 강민석
date: 2021-03-15 01:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 217 - Contains Duplicate 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/contains-duplicate/>  

![](/assets/img/sample/leetcode/217/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/217/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU>에서 추천한 문제입니다.  


-----  

## 4. 문제 해석

- 배열이 주어집니다. 최소 2번 이상 반복되는 요소가 있으면 True 없으면 False를 리턴합니다.

-----  

## 5. code

**python**

```python

class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        dic = {}
        for i in range(len(nums)) : 
            temp = dic.get(nums[i],2)
            if temp != 2 :
                return True
            dic[nums[i]] = 1
        return False
```

-----  

**c++**

```c++

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_map<int,bool> map;
        for(size_t i = 0;i<nums.size();++i)
        {
            if(map[nums[i]]==true)
                return true;
            map[nums[i]]=true;
        }
        return false;
    }
};

```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 86% python 99%**  
![](/assets/img/sample/leetcode/155/result.JPG)  



 
**c++99% 8ms code**

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = 0; i < n - 1; i++)
            if (nums[i] == nums[i+1])
                return true;
        return false;
    }
};
```
정렬하고 끝요소까지 같은 요소가 없으면 false를 리턴합니다.