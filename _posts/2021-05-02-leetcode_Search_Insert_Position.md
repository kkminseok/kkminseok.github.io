---
title: leetcode(리트코드)35-Search Insert Position
author: 강민석
date: 2021-05-02 07:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 35 - Serach Insert Position 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/search-insert-position/> 

![](/assets/img/sample/leetcode/35/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/35/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Random으로 뽑아서 푼 문제입니다. 


-----  

## 4. 문제 해석

- 정렬된 벡터가 들어옵니다.
- 타겟이 있으면 해당 인덱스를 리턴하고, 없으면 정렬 시 들어갈 인덱스를 찾아 리턴합니다.



-----  

## 5. code  

코드설명
- target이 이미 정렬된 벡터의 끝요소보다 클 경우를 먼저 리턴하는게 좀 더 빠릅니다.

**c++**

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        if(target > nums[nums.size()-1]) return nums.size();
        for(int i = 0 ; i <nums.size();++i){
            if(nums[i] >= target) return i;
        }
        return nums.size();
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 100% 0ms**

![](/assets/img/sample/leetcode/35/result.JPG)  



