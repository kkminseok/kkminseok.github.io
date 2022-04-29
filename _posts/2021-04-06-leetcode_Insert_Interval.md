---
title: leetcode(리트코드)57-Insert Interval
author: 강민석
date: 2021-04-06 01:45:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 57 - Insert Interval 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/insert-interval/>  

![](/assets/img/sample/leetcode/57/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/57/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- 문제가 직관적입니다.


-----  

## 5. code


**c++**


```c++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> res;
        int index = 0 ;
        while(index < intervals.size() && intervals[index][1] < newInterval[0]){
            res.push_back(intervals[index++]);
        }
        while(index < intervals.size() && intervals[index][0] <= newInterval[1]){
            newInterval[0] = min(newInterval[0],intervals[index][0]);
            newInterval[1] = max(newInterval[1],intervals[index][1]);
            ++index;
        }
        res.push_back(newInterval);
        while(index < intervals.size()){
            res.push_back(intervals[index++]);
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!


**c++ 16ms 60%**


![](/assets/img/sample/leetcode/57/result.JPG)  








