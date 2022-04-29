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

코드설명
- 갯수를 세주는데 가장 많이 들어온 값을 count 변수에 넣어줍니다.
- 어차피 가장 많이 들어온 값의 task가 끝나지 않으면 문제는 안풀리기에 가장 많이 들어온 값을 기준으로 task를 진행합니다.
- 마지막 task를 수행하기 전에 count값이 같은 알파벳의 갯수를 세주고 그 알파벳을 세워주면 되므로 result값이 1씩 더해줍니다.
- 예를 들어 마지막 예제인 A가 6개, B,C,D,E,F,G가 들어오면 가장 많이 들어온 값인 A의 count(6개) - 1인 5개가 끝나기 전에는 해당 문제는 풀리지 않으므로 5개를 세워주고 count가 같은 다른 알파벳이 있는 지 찾습니다.
- 찾은 알파벳만큼 뒤에 세워주면 결과값이 나오는 겁니다.
- 즉 A ~~(3개 아무거나 때움), A~~,A~~,A~~,A~~,AB 이런식으로 진행됩니다.


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



