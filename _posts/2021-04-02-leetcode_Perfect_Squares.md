---
title: leetcode(리트코드)279-Perfect Squares
author: 강민석
date: 2021-04-02 02:04:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 279 - Perfect Squares 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/perfect-squares/>  

![](/assets/img/sample/leetcode/279/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/279/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 최소한의 거듭제곱을 사용하여 n을 만들어야 합니다.
- 최소한의 경우의 수를 리턴합니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int exp(int n){
        int i = 2;
        while(!(pow(i,2)<= n && pow(i+1,2)>n)){
            ++i;
        }
        return i;
    }
    int numSquares(int n) {
        vector<int> DP(n+4,987);
        DP[1]=1;
        DP[2]=2;
        DP[3]=3;
        if(n<=3)return DP[n];
        for(int i = 4 ;i<=n;++i){
            //거듭제곱 지수 찾기
            int findexp = exp(i);
            if(pow(findexp,2)==i) DP[i] = 1;
            else{
                while(findexp>1)
                {
                    int expr = pow(findexp,2);
                    DP[i] = min(DP[expr] + DP[i - expr], DP[i]);
                    --findexp;
                }
            }
        }
        return DP[n];
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

실행속도가 빠른 코드는 수학적으로 풀었는데 제가 해석을 못하여 첨부하지는 않겠습니다.





