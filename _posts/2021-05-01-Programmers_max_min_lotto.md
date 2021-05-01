---
title: Programmers_로또의 최고 순위와 최저 순위
author: 강민석
date: 2021-05-01 13:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 로또의 최고 순위와 최저 순위 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/77484>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Dev-Matching 백엔드 코테 문제입니다.
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적이고 어렵지 않은 문제입니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <set>
#include <iostream>
using namespace std;

vector<int> solution(vector<int> lottos, vector<int> win_nums) {
    vector<int> answer;
    //set에 넣어서 접근할 수 있게 만듭니다.
    set<int> ans(win_nums.begin(),win_nums.end());
    int max = 1 ;
    int min = 1;
    for(int i = 0; i<lottos.size();++i){
        //0인 경우에는 최저 순위만 올라갑니다(틀렸다고 가정)
        if(lottos[i] ==0 ){
            ++min;
        }
        else{
            //만약 찾을 수 없다면 둘 다 순위가 올라갑니다.
            if(!ans.count(lottos[i])){
                ++max;
                ++min;
            }
                
        }
    }
    //만약 다 없거나 다 맞춘 경우 min값이나 max값이 6을 초과하므로 다시 맞춰줍니다.
    min =  min >6 ? 6 : min;
    max = max > 6 ? 6 :max;
    answer.push_back(max);
    answer.push_back(min);
    return answer;
}
```

-----



## 5. 결과

필요시.














 
