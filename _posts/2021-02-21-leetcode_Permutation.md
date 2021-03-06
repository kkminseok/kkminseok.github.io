---
title: leetcode(리트코드)46-Permutations
author: 강민석
date: 2021-02-21 14:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 46 - Permutations 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/permutations/>  

![](/assets/img/sample/leetcode/46/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/46/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- Permutation을 구합니다.

-----  

## 5. code

```c++
class Solution {
public:
    
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        do{
            vector<int> temp;
            for(int i =0;i<nums.size();++i)
                temp.push_back(nums[i]);
            result.push_back(temp); 
        }while(next_permutation(nums.begin(),nums.end()));
        
        
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/46/result.JPG)  

next_permutation 함수를 잊지말자. 정렬을 먼저 해줘야 올바르게 나옴.






