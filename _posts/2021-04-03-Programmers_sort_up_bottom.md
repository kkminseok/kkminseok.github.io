---
title: Programmers_정수 내림차순으로 배치하기
author: 강민석
date: 2021-04-03 10:33:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 정수 내림차순으로 배치하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12933>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않습니다.



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

long long solution(long long n) {
    long long answer = 0;
    string temp = to_string(n);
    sort(temp.begin(),temp.end(),greater<char>());
    return answer = stol(temp);
}
```

-----

## 5. 결과

필요시.














 
