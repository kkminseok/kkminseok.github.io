---
title: Programmers_음양 더하기
author: 강민석
date: 2021-05-01 14:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 음양 더하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/76501>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌 2문제입니다.
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

int solution(vector<int> absolutes, vector<bool> signs) {
    int answer = 0;
    for(int i = 0 ; i <absolutes.size();++i){
        //음수인 경우 빼줍니다.
        if(!signs[i]){
            answer -= absolutes[i];
        }
        //양수인 경우 더해줍니다.
        else
            answer += absolutes[i];
    }
    return answer;
}
```

-----



## 5. 결과

필요시.














 
