---
title: Programmers_평균 구하기
author: 강민석
date: 2021-04-03 12:33:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 평균 구하기 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12944>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>

using namespace std;

double solution(vector<int> arr) {
    if(arr.size()==1)return arr[0];
    double answer = 0;
    for(size_t i = 1 ;i<arr.size();++i)
        arr[i] +=arr[i-1];
    answer = arr[arr.size()-1] / double(arr.size());
    return answer;
}
```

-----

## 5. 결과

필요시.














 
