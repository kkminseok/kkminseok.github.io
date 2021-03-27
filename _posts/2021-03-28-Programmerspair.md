---
title: Programmers_짝지어제거하기
author: 강민석
date: 2021-03-28 00:00:00 +0800
categories: [Algorithm]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 짝지어 제거하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12973>
![](/assets/img/sample/Programmers/pair.JPG)  




-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
지인들이 풀어보라해서 풀어봤습니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 문자열의 길이가 백만으로 O(n)으로 풀어야한다는 사실을 알았습니다. 매 번 문자열을 자르기보다 도미노처럼 쌓은걸 부셔버린다? 라는 생각으로 스택으로 풀었습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <iostream>
#include<string>
#include<stack>
using namespace std;

int solution(string s)
{
    stack<char> st;
    for(size_t i =0;i<s.size();++i)
    {
        if(st.size()==0) st.push(s[i]);
        else
            if(st.top() == s[i]) st.pop();
            else
                st.push(s[i]);
        
    }
    return st.empty();
}
```
-----

## 5. 결과














 
