---
title: Programmers_이상한 문자 만들기
author: 강민석
date: 2021-04-06 13:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 이상한 문자 만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12930>






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

string solution(string s) {
    int count = 0 ;
    for(size_t i = 0 ;i<s.size();++i){
        if(s[i] == ' '){
            count=0;
            continue;
        }
        if(count==0 || count%2==0)
            s[i] = toupper(s[i]);
        else if(count%2==1)
            s[i] = tolower(s[i]);
        ++count;
    }
    return s;
}
```

-----



## 5. 결과

필요시.














 
