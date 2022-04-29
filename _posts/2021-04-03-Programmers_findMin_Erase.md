---
title: Programmers_제일 작은 수 제거하기
author: 강민석
date: 2021-04-03 12:34:13 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 제일 작은 수 제거하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12935>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.
- O(n)으로 빠르게 제거하는 코드입니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<int> arr) {
    auto min = arr.begin();
    for(auto it = arr.begin(); it!=arr.end(); ++it){
        if(*min > *it) min = it;
    }
    arr.erase(min);
    if(arr.size()==0){
        arr.push_back(-1);
    }
    
    return arr;
}
```

-----

## 5. 결과

필요시.














 
