---
title: Programmers_카카오2018blind - 다트게임
author: 강민석
date: 2021-03-31 12:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakaoblind2018]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 다트게임 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17682>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2018년도 kakao Blind 문제이고, 난이도는 가장 쉬운 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 이전 점수에 대한 정보가 필요하므로 벡터나 배열에 값을 저장해야한다고 생각했습니다.
- 어차피 숫자 뒤에는 문자가 한 개는 꼭 와야하므로 숫자에 대한 정보를 확인한 뒤 배열범위 검사는 해주지 않았습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include<vector>
#include<iostream>
using namespace std;

int solution(string dartResult) {
    int answer = 0;
    vector<int> score(3, 0);
    int vin = 0;
    int i = 0;
    for (size_t i = 0; i < dartResult.size();)
    {
        if (dartResult[i] >= 48 && dartResult[i] <= 57) {
            string temp = "";
            temp += dartResult[i];
            if (dartResult[i + 1] == '0') {
                temp += '0';
                ++i;
            }
            score[vin++] = stoi(temp);
            ++i;
        }
        if (dartResult[i] == 'D') {
            score[vin-1] = score[vin-1] * score[vin-1];
        }
        else if (dartResult[i] == 'T') {
            score[vin-1] = score[vin-1] * score[vin-1] * score[vin-1];
        }
        if (dartResult[i] == '*') {
            score[vin-1] *= 2;
            if (vin-1 != 0)
                score[vin - 2] *= 2;
        }
        else if (dartResult[i] == '#') {
            score[vin-1] *= (-
                             .1);
        }
        ++i;
    }
    for(int i = 0;i<3;++i){
        answer += score[i];
    }
    return answer;
}
```
-----

## 5. 결과

필요시.














 
