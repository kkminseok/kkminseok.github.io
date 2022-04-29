---
title: Programmers_문자열 내 마음대로 정렬하기
author: 강민석
date: 2021-04-06 12:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 문자열 내 마음대로 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12915>






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
#include <iostream>
using namespace std;


vector<string> solution(vector<string> strings, int n) {
    if(strings.size()==1)
        return strings;
    for(size_t i = 0 ; i<strings.size()-1; ++i){
        for(size_t j = i+1;j<strings.size();++j){
            if(strings[i][n] > strings[j][n])
                swap(strings[i],strings[j]);
            else if(strings[i][n]==strings[j][n]){
                if(strings[i]>strings[j])
                    swap(strings[i],strings[j]);
            }            
        }
            
    }
    return strings;
}
```

-----



## 5. 결과

필요시.














 
