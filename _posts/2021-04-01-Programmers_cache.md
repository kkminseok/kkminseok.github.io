---
title: Programmers_카카오2018blind - 캐시
author: 강민석
date: 2021-04-01 12:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakaoblind2018]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 캐시 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17680>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2018년도 kakao Blind 문제이고, 난이도는 가장 쉬운 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- LRU 알고리즘을 사용했을 때 걸리는 시간을 리턴하는 간단한 문제입니다.



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<unordered_map>
#include<list>
#include<cctype>
using namespace std;

//대소문자 구분을 안하므로 모든 영어를 소문자로 바꿔줍니다.
string changelower(string str){
    for(size_t i =0;i<str.size();++i) str[i] = tolower(str[i]);
    return str;
}

int solution(int cacheSize, vector<string> cities) {
    //LRU 구현
    int answer = 0;
    list<string> dq;
    unordered_map<string,list<string>::iterator> um;
    
    for(size_t i =0;i<cities.size();++i){
        string putstr = changelower(cities[i]);
        if(cacheSize==0){
            answer+=5;
            continue;
        }
        if(um.find(putstr) == um.end()){
            answer+=5;
            if(dq.size() == cacheSize){
                string last = dq.back();
                dq.pop_back();
                um.erase(last);
            }
        }
        else{
            answer+=1;
            dq.erase(um[putstr]);
        }
        
        dq.push_front(putstr);
        um[putstr] = dq.begin();
    }
    
    return answer;
}
```

-----

## 5. 결과

필요시.














 
