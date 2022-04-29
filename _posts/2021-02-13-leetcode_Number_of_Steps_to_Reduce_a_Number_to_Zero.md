---
title: leetcode(리트코드)2월12일 challenge1342-Number of Steps to Reduce a Number to Zero
author: 강민석
date: 2021-02-13 13:30:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge12 - Number of Steps to Reduce a Number to Zero 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/number-of-steps-to-reduce-a-number-to-zero/>  

![](/assets/img/sample/leetcode/1342/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1342/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월12일자 챌린지 문제입니다.   

-----  

## 4. 문제 해석

- 주어진 값이 0으로 향하면서 짝수인 경우는 2로 나눠주고 홀수인 경우는 1을 빼줍니다. 그 과정이 몇번 있는 지 값을 계산하여 return 해줍니다.  

-처음에는 DP로 풀었는데, 시간이 너무 오래걸려서 BruteForce로 풀었습니다.  


-----  

## 5. code

**DP 코드**


```c++
const int MAX = 1000001;
class Solution {
public:

    int DP[MAX]={0,};
    int numberOfSteps (int num) {
        DP[1] = 1;
        DP[2] = 2;
        for(int i = 3;i<MAX;++i)
        {
            if(i%2==0)
            {
                DP[i] = DP[i/2]+1;
            }
            else
            {
                DP[i] = DP[i-1] + 1;
            }
        }
        return DP[num];
    }
};
```

**고친 코드**

```c++
class Solution {
public:
    int numberOfSteps (int num) {
     int count = 0;
     while(num!=0)
     {
         if(num%2==0)
         {
             count++;
             num/=2;
         }
         else
         {
             count++;
             num-=1;
         }
     }
    return count;
    }
};
```

-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/1342/result.JPG) 

