---
title: Programmers_카카오2018Blind[1차] - 뉴스 클러스터링
author: 강민석
date: 2021-04-02 01:23:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakaoblind2018]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 뉴스 클러스터링 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17677>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2018년도 kakao Blind 1차 문제이고, 난이도는 level2 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제에서 합집합을 구할 때 max 어쩌구해서 복잡하게 구하는데 수학시간에 합집합은 A 집합의 크기  + B 집합의 크기 - 교집합의 크기로 구한다고 배웠습니다. 이렇게 구하는게 더 쉬웠습니다.
- 접근하기 쉬운 map에 각각 원소를 넣으면서 이미 존재하면 count값을 올려주고 없으면 1로 초기화 해줍니다. 넣으면서 집합의 크기를 count해줘야합니다.
- isalpha()함수를 통해 영어인지 확인해주었고 tolower()함수를 통해 소문자로 일관성을 주었습니다.



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <unordered_map>
#include <cctype>
#include <iostream>
#include <cmath>
using namespace std;

int solution(string str1, string str2) {
    int answer = 0;
    unordered_map<string,int> mtm1;
    unordered_map<string,int> mtm2;
    
    //합집합 크기
    int sum = 0;
    //교집합 크기
    int intersection = 0;
    
    //str1부터
    for(size_t i = 0;i<str1.size()-1;++i){
        // 영어이면 map에 넣습니다.
        if(isalpha(str1[i]) && isalpha(str1[i+1])){
            string tempstr = "";
            tempstr+=tolower(str1[i]);
            tempstr += tolower(str1[i+1]);
            if(mtm1.count(tempstr)) mtm1[tempstr]++;
            else
                mtm1[tempstr] = 1;
            ++sum;
        }
    }
    //str2
    for(size_t i = 0;i<str2.size()-1;++i){
        if(isalpha(str2[i]) && isalpha(str2[i+1])){
            string tempstr = "";
            tempstr+=tolower(str2[i]);
            tempstr += tolower(str2[i+1]);
            if(mtm2.count(tempstr)) mtm2[tempstr]++;
            else
                mtm2[tempstr] = 1;
            ++sum;
        }
    }
    //map에 한 개도 안들어와있으면 65536을 리턴
    if(mtm1.empty() && mtm2.empty()) return 65536;
    
    
    // map1을 돌면서 map2에 똑같은게 있으면 더 작은값(교집합)을 intersection에 더해줍니다.
    for(auto it = mtm1.begin(); it!=mtm1.end();++it){
        string str = it->first;
        if(mtm2.find(str) != mtm2.end()){
            intersection += min(mtm1[str],mtm2[str]);
        }
    }
    //합집합 구하기
    sum -=intersection;
    //연산을 위해 형 변환
    double gob = intersection / (double)sum * 65536;
    //소수점 버림.
    gob = trunc(gob);
    answer=gob;
    
    return answer;
}
```

-----

## 5. 결과

필요시.














 
