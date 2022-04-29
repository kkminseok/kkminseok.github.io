---
title: Programmers_카카오2021blind - 신규 아이디 추천
author: 강민석
date: 2021-04-06 09:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakaoblind2021]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 신규 아이디 추천 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/72410>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2021년도 kakao Blind 문제이고, 난이도는 가장 쉬운 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 그리디하게 풀면 됩니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <iostream>
using namespace std;

void one(string& str){
    for(int i = 0 ; i<str.size();++i){
        if(isalpha(str[i]))
            str[i] = tolower(str[i]);
        //2
        if(!isalpha(str[i])  && !isdigit(str[i]) && str[i] != '-' && str[i] !='_' && str[i] !='.')
        {
            str.erase(i,1);
            --i;
        }
        //3
        if(i+1<str.size() && str[i] == '.' && str[i+1]=='.')
        {
            str.erase(i,1);
            --i;
        }
    }
}

void eraselast(string& str){
    if(str[str.size()-1] =='.')
        str.erase(str.size()-1,1);
}

string solution(string new_id) {
    string answer = "";
    //1,2,3
    one(new_id);
    //4
    if(new_id[0] =='.')
        new_id.erase(0,1);
    eraselast(new_id);
    //5
    if(new_id.empty())
        new_id += 'a';
    //6
    if(new_id.size()>=16){
        new_id = new_id.substr(0,15);
        eraselast(new_id);
    }
    else if(new_id.size()<=2){
        while(new_id.size()!=3)
            new_id += new_id[new_id.size()-1];
    }
    
    return answer = new_id;
}
```

-----

## 5. 결과

필요시.














 
