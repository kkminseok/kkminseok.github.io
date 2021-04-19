---
title: leetcode(리트코드)621-Task Scheduler
author: 강민석
date: 2021-04-19 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 621 - Task Scheduler 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/task-scheduler/> 

![](/assets/img/sample/leetcode/621/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/621/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- tasks가 주어집니다. n은각 tasks를 처리하는 시간이라고 생각하면 됩니다.
- 이미 같은 문자의tasks가 이미 처리중일 때 idle을 통해 해당 tasks가 끝날 때까지 기다려야합니다.
- 최소한의 시간으로 tasks들을 수행할 때 그 시간을 리턴하세요.



-----  

## 5. code


**c++**


```c++
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char,int> um;
        int count = 0 ; 
        for(size_t i =0 ;  i <tasks.size();++i){
            um[tasks[i]] ++;
            count = max(um[tasks[i]],count);
        }
        int res = (count -1) * (n+1);
        for(auto e : um){
            if(e.second == count) ++res;
        }
        if(tasks.size() >= res) res = tasks.size();
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 20% 116ms**

![](/assets/img/sample/leetcode/621/result.JPG)  



