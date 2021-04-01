---
title: Programmers_월간 코드 챌린지 - 두 개 뽑아서 더하기
author: 강민석
date: 2021-04-01 15:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,codechallenge]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 두 개 뽑아서 더하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/68644>



-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 배열 안에 있는 요소를 2개 뽑아서 만들 수 있는 모든 경우의 수의 값을 구해 리턴합니다.
- 중복은 제외해야합니다 -> set을 쓴 이유


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <unordered_set>
#include <algorithm>
using namespace std;

vector<int> solution(vector<int> numbers) {
    vector<int> answer; 
    unordered_set<int> s;
    for(size_t i = 0;i<numbers.size();++i){
        for(size_t j = i+1;j<numbers.size();++j)
        {
            int sum = numbers[i] + numbers[j];
            if(s.count(sum)) continue;
            s.insert(sum);
            answer.push_back(sum);
        }
    }
    sort(answer.begin(),answer.end());
    return answer;
}
-----

## 5. 결과

필요시.














 
