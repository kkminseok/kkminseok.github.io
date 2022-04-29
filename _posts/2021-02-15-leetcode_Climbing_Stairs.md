---
title: leetcode(리트코드)70-Climbing Stairs
author: 강민석
date: 2021-02-15 14:10:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 70 - Climbing Stairs문제입니다.**

## 1. 문제
<https://leetcode.com/problems/climbing-stairs/>  

![](/assets/img/sample/leetcode/70/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/70/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 간단한 DP 문제입니다. 1step과 2step으로 계단을 올라갈 때 n에 도달하는 경우의 수 입니다.


-----  

## 5. code

```c++
class Solution {
public:
    int climbStairs(int n) {
        int DP[46]={0,};
        if(n==1)
            return 1;
        DP[0]=1;
        DP[1]=1;
        for(int i=2;i<=n;++i)
        {
            DP[i]= DP[i-2] + DP[i-1];
        }
        return DP[n];
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(67%)**  

![](/assets/img/sample/leetcode/70/result.JPG)


위의 점화식은

n=4라고 하면DP[i-2]는 DP[2]로 (1+1, 2)라는 경우의 수가 저장되어 있습니다. DP[i-1]은 DP[3]으로 (1+1+1,2+1,1+2)라는 경우의 수가 들어있습니다.  

DP[2]의 경우의 수에 +2를 해준 경우의 수 (1+1+2,2+2)와  
DP[3]의 경우의 수에 +1를 해준 경우의 수 (1+1+1+1,2+1+1,1+2+1)의 경우의 수를 합친 5가 DP[4]가 됩니다.  
