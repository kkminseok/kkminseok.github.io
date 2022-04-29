---
title: Programmers_Summer/Winter Coding(~2018) - 소수 만들기
author: 강민석
date: 2021-04-03 01:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 소수만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12977>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Level 1난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 주어진 벡터에서 요소를 3개 뽑아 소수를 만들 수 있는지 확인합니다.
- 1000이하의 자연수가 50 최대 50번 나올 수 있으므로(중복 없다고 했지만 그냥 넉넉하게) 1000 * 50의 사이즈를 가진 벡터를 만들었습니다.
- 에라토스테네스의 체를 이용하였습니다.



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <vector>
#include <iostream>
#include <cmath>
using namespace std;

int solution(vector<int> nums) {
    int answer = 0;
    //소수 벡터
    const int size = 50000;
    vector<int> sosu(size,0);
    for(int i = 2; i < size ; ++i)
        sosu[i] = i;
    
    for(int i = 2 ; i<=sqrt(size);++i){
        if(sosu[i]==0) continue;
        for(int j = 2*i; j<=size;j+=i){
            sosu[j] = 0;
        }
    }
    for(int i = 0;i<nums.size()-2;++i){
        for(int j = i+1; j <nums.size()-1;++j){
            for(int k = j+1; k <nums.size();++k)
                if(sosu[nums[i] + nums[j] + nums[k]] !=0)
                    ++answer;
        }
    }

    

    return answer;
}
```

-----

## 5. 결과

필요시.














 
