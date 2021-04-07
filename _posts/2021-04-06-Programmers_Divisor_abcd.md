---
title: Programmers_나누어 떨어지는 숫자 배열
author: 강민석
date: 2021-04-06 05:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 나누어 떨어지는 숫자 배열 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12910>






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
#include <algorithm>
using namespace std;

vector<int> solution(vector<int> arr, int divisor) {
    vector<int> answer;
    for(size_t i = 0 ; i<arr.size();++i){
        if(arr[i]%divisor == 0 ) answer.push_back(arr[i]);
    }
    if(answer.empty()) answer.push_back(-1);
    sort(answer.begin(),answer.end());
    return answer;
}
```

-----



## 5. 결과

필요시.














 
