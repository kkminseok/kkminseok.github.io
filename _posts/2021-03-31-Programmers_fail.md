---
title: Programmers_카카오blind2019 - 실패율
author: 강민석
date: 2021-03-31 13:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakaoblind2019]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 실패율 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/42889#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2019년도 kakao Blind 문제이고, 난이도는 가장 쉬운 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 정렬된 상태로 푸는게 쉽다고 생각했습니다. 이유는 스테이지를 점차적으로 증가시키고 나중에 실패율로 기준으로 정렬을 다시하려고 했기 때문입니다.
- 테케의 4,4,4,4,4 같은 경우 때문에 순차적으로 인덱스를 접근하는 방식은 무리가 있다고 생각하여 multiset 자료구조를 사용하였습니다.
- 0으로 나눠주는 경우를 조심하면 됩니다.


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
#include<iostream>
#include<set>
using namespace std;

//실패율을 기준으로 정렬해주기 위한,
bool pred(pair<int, double>& a, pair<int, double>& b) {
    // 실패율이 같으면 인덱스가 작은 순으로.
    if (a.second == b.second)
        return a.first < b.first;
    return a.second > b.second;
}

vector<int> solution(int N, vector<int> stages) {
    vector<int> answer;
    //인덱스와 실패율 값을 저장할 자료구조
    vector < pair<int, double>> DP;
    //알아서 정렬되도록, count()함수를 이용하여 갯수를 구함.
    multiset<int> st;
    for (size_t i = 0; i < stages.size(); ++i) {
        st.insert(stages[i]);
    }
    int stage = 1;
    int stagesize = stages.size();
    while (stage<=N) {
        
        double count = st.count(stage);
        double result;
        //0으로 나눠주는 경우를 방지
        if(stagesize==0)
            result = 0;
        else
            result = count/stagesize;
        DP.push_back(make_pair(stage, result));
        stagesize -= count;
        ++stage;
    }
    //재 정렬
    sort(DP.begin(), DP.end(), pred);
    for (int i = 0; i < DP.size(); ++i) {
        //cout<<DP[i].first<<" "<<DP[i].second<<'\n';
        answer.push_back(DP[i].first);
    }
    return answer;
}
```
-----

## 5. 결과

필요시.














 
