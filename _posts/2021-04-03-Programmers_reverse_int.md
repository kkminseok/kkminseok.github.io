---
title: Programmers_자연수 뒤집어서 배열로 만들기
author: 강민석
date: 2021-04-03 00:01:01 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 자연수 뒤집어서 배열로 만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12932>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- no hard so eazy



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(long long n) {
    vector<int> answer;
    while(n!=0){
        answer.push_back(n%10);
        n/=10;
    }
    return answer;
}
```

-----

## 5. 결과

필요시.














 
