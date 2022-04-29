---
title: Programmers_월간 코드 챌린지 시즌1 - 3진법 뒤집기
author: 강민석
date: 2021-04-06 15:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 3진법 뒤집기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/68935>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌1 문제입니다. 가장 쉬운 난이도 입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- string을 써서 쉽게 풀었습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <string>
#include <iostream>
#include <cmath>
using namespace std;

int solution(int n) {
    int answer = 0;
    string str = "";
    while(n!=0){
        str += (n%3) + 48;
        n/=3;
    }
    int exp = 0;
    for(int i = str.size()-1; i>=0;--i){
        answer += pow(3,exp++) * (str[i]-48);
    }
    return answer;
}
```

-----

## 5. 결과

필요시.














 
