---
title: Programmers_카카오blind2018 - 비밀지도
author: 강민석
date: 2021-03-31 14:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakaoblind2018]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 비밀지도 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17681>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2018년도 kakao Blind 문제이고, 난이도는 가장 쉬운 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제 난이도자체는 어렵지 않습니다. 효율성이 관건이라 생각하여 어떻게 하면 빨리 풀 지 생각했습니다.
- 하나의 for문에서 최대한 문제를 해결하려고 했습니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<algorithm>

using namespace std;

//2진수로 바꿔버리는 함수. 자릿수가 최대 16자리가 되므로 int로 바꾸는게 아닌 string으로 바꿔야함.
string maketwo(int num){
    if(num==0)
        return "0";
    string making = "";
    while(num!=1){
        making+=(num%2) +48;
        num/=2;
    }
    making+='1';
    return making;
}

vector<string> solution(int n, vector<int> arr1, vector<int> arr2) {
    vector<string> answer;
    for(size_t i =0;i<n;++i){
        //이진수로 바꿈
        string first = maketwo(arr1[i]);
        //만약 이진수로 바꿨는데 자릿수가 안맞으면 0을 추가
        while(first.size()!=n)
            first+='0';
        //이진수로 바꿈
        string two = maketwo(arr2[i]);
        while(two.size()!=n)
            two+='0';
        //만약n이 5이고 1의 경우 위의코드까지만 했을 땐 10000이므로 reverse인 00001로 바꿔줌.   
        reverse(first.begin(),first.end());
        reverse(two.begin(),two.end());
        
        string indexanswer= "";
        //바꾼 문자열의 인덱스에서 1이 하나라도 있으면 #을 넣어줍니다.
        for(size_t j =0;j<n;++j){
            if(first[j]== '1' || two[j] == '1'){
                indexanswer += "#";
            }
            else
                indexanswer += " ";
        }
        answer.push_back(indexanswer);
    }
    return answer;
}
-----

## 5. 결과

필요시.














 
