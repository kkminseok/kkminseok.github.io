---
title: Programmers_124 나라의 숫자
author: 강민석
date: 2021-05-02 13:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 124 나라의 숫자 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12899#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않지만 나누기 처리를 잘 해줘야합니다.
- 나누어 떨어지는 경우의 처리가 중요합니다.

-----  

## 4. 접근 방법을 적용한 코드


```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string solution(int n) {
    string answer = "";
    char arr[2] = {'1','2'};
    while(n){
        //나누어 떨어지는 경우 --n을 해줘야합니다.
        if(n%3 ==0){
            answer.push_back('4');
            --n;
        }
        else{
            answer.push_back(arr[n%3-1]);
        }
        n/=3;       
    }
    //뒤부터 집어넣었으므로 마지막에 뒤집어 줘야합니다.
    reverse(answer.begin(),answer.end());
    return answer;
}
```


-----



## 5. 결과

필요시.














 
