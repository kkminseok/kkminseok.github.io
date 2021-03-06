---
title: leetcode(리트코드)2월21일 challenge991-Broken Calculator
author: 강민석
date: 2021-02-21 16:01:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge991 - Broken Calculator 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/broken-calculator/>  

![](/assets/img/sample/leetcode/991/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/991/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월21일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- X에서 2를 곱하거나 -1을 하여 Y로 만들어야합니다. 카운트를 리턴합니다.
- 백트래킹으로 Y를 X로 만드는 것에 신경을 썼습니다.

-----  

## 5. code

```c++
class Solution {
public:
    
    int brokenCalc(int X, int Y) {
        queue<pair<int,int>> q;
        q.push(make_pair(Y,0));
        if(Y<X)
            return X-Y;
        while(!q.empty())
        {
            int now = q.front().first;
            int count = q.front().second;
            q.pop();
            if(now<X)
                return count+X-now;
            if(now == X)
                return count;
            if(now%2==0)
                q.push(make_pair(now/2,count+1));
            else
                q.push(make_pair(now+1,count+1));

        }
        return 0;
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/991/result.JPG)  


**더 간단한 코드**

```c++
class Solution {
public:
    int brokenCalc(int x, int y) {
        if(x == y)
        {
            return 0;
        }
        if(x > y)
        {
            return x-y;
        }
        if(y %2 == 0)
        {
            return 1+brokenCalc(x , y/2);
        }
        else
        {
            return 1 + brokenCalc(x , y+1);
        }
        
    }
};
```

코드 본문의 내용이 쉬우므로 해설은 적지 않겠습니다.  

