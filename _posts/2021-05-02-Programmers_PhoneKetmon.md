---
title: Programmers_폰켓몬
author: 강민석
date: 2021-05-02 11:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 폰켓몬 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/1845>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
찾아라 프로그래밍 마에스터 문제입니다.
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.

-----  

## 4. 접근 방법을 적용한 코드


```c++
#include <vector>
#include <set>
#include <algorithm>
using namespace std;

int solution(vector<int> nums)
{
    set<int> st;
    for(int i = 0 ; i <nums.size();++i){
        st.insert(nums[i]);
    }
    return min((int)st.size(),(int)(nums.size()/2));
}
```


-----



## 5. 결과

필요시.














 
