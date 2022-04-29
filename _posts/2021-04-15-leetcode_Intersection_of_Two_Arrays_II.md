---
title: leetcode(리트코드)350-Intersection of Two Arrays II
author: 강민석
date: 2021-04-15 11:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 350 - Intersection of Two Arrays II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/intersection-of-two-arrays-ii/>  

![](/assets/img/sample/leetcode/350/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/350/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 두 개의 집합에서의 교집합을 리턴합니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        //첫 벡터를 담을 맵
        unordered_map<int,int> um1;
        //두 번째 벡터를 담을 맵
        unordered_map<int,int> um2;
        //결과 벡터
        vector<int> res;
        //값들을 맵에 넣습니다.
        for(size_t i = 0 ; i< nums1.size();++i)
            um1[nums1[i]]++;
        for(size_t i = 0 ; i <nums2.size();++i)
            um2[nums2[i]]++;
        
        //교집합이므로 um1으로 돌든 um2로 돌든 상관은 없습니다.
        for(auto it = um1.begin(); it!=um1.end(); ++it){
            //두 맵에 값이 존재하면
            if(um1.count(it->first) && um2.count(it->first)){
                //더 작은 값을 기준으로 카운트해주고 벡터에 넣어줍니다.
                int count = min(um1[it->first] , um2[it->first]);
                for(int i = 0; i <count;++i)
                    res.push_back(it->first);
            }
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 40% python ??%** 
![](/assets/img/sample/leetcode/350/result.JPG)  






