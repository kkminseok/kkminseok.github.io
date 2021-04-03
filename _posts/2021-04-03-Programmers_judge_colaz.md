---
title: Programmers_콜라츠 추측
author: 강민석
date: 2021-04-03 05:12:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 콜라츠 추측 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12943#qna>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.
- 받아온 int를 그대로 쓸 경우 오버플로가 날 수 있습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>

using namespace std;

int solution(int num) {
    long long tempn = num;
    int answer = 0;
    while(answer<=500 && tempn!=1){
        if(tempn%2 ==0)tempn/=2,answer++;
        else tempn=(tempn * 3 + 1),answer++;
    }
    return answer == 501 ? -1 : answer;
}

-----
```


## 5. 결과

필요시.














 
