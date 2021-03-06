---
title: Programmers_DP04 - 도둑질
author: 강민석
date: 2021-02-04 15:03:00 +0800
categories: [Algorithm,DP]
tags: [Programmers,DP]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 도둑질 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/DP04/Problem.JPG)  



-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 4의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 비슷한 문제가 백준에 있습니다. 
- 주의할 점은 처음 집을 들리면 맨 마지막 집을 들리면 안된다는 점입니다. 이부분만 잘 해결해주면 됩니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
#include<iostream>
using namespace std;

int dp[1000001] = { 0, };
int dp2[1000001] = { 0, };
int solution(vector<int> money) {
    int answer = 0;
    //첫번째를 잘 선택해야함 
    dp[0] = money[0];
    dp[1] = money[0];
    //첫번째 집을 안 드르는 경우
    dp2[0] = 0;
    dp2[1] = money[1];
    for (size_t i = 2; i < money.size(); ++i)
    {
        if(i!=money.size()-1)//마지막은 들어가면 안됨.
            dp[i] = max(dp[i - 2] + money[i], dp[i - 1]);
        dp2[i] = max(dp2[i - 2] + money[i], dp2[i - 1]);
    }
    answer = max(dp[money.size() - 2],dp2[money.size()-1]);
    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/DP04/result.JPG)  













 
