---
title: leetcode(리트코드)3월23일 challenge923-3Sum With Multiplicity
author: 강민석
date: 2021-03-23 00:03:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 23일 - 3Sum With Multiplicity 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/3sum-with-multiplicity/>  

![](/assets/img/sample/leetcode/923/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/923/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 23일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 숫자 세개를 더해서 target을 만들 수 있는 경우의 수를 모두 찾아 10^+7로 나눈 나머지를 리턴합니다.


-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int threeSumMulti(vector<int>& arr, int target) {
        unordered_map<int,long long> um;
        for(size_t i = 0;i<arr.size();++i)
            um[arr[i]]++;
        
        long long result=0;
        
        for(auto it : um)
        {
            for(auto it2: um)
            {
                int first =it.first;
                int second = it2.first;
                int third = target-first-second;
                if(!um.count(third))continue;
                
                if(first == second && second== third)
                    result += um[first]* (um[first]-1) * (um[first]-2) / 6;
                else if(first==second && second!=third)
                    result += (um[first] * (um[first]-1))/2 * um[third];
                else if(first<second && second<third)
                    result += um[first] * um[second] * um[third];
                
            }
        }
        return (result %(int)(1e9+7));
    }
};


```

-----

## 6. 결과 및 후기, 개선점

**c++ 84%**
![](/assets/img/sample/leetcode/923/result.JPG)  

