---
title: Programmers_카카오2020 인턴십 - 키패드 누르기
author: 강민석
date: 2021-04-06 15:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakao2020ship]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 키패드 누르기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/67256>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2020년도 kakao 인턴십 문제이고, 난이도는 가장 쉬운 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 두 점 사이의 거리를 구해서 푸는 문제가 아닙니다.




-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <iostream>
#include <utility>
#include <cmath>
using namespace std;

string solution(vector<int> numbers, string hand) {
    string answer = "";
    //왼손 *
    pair<int,int> left = make_pair(3,0);
    //오른손 #
    pair<int,int> right = make_pair(3,2);
    
    for(size_t i = 0 ;i<numbers.size();++i){
        //0일 때는 넘겨줍니다.
        //왼손 담당
        if( numbers[i]!=0 && numbers[i]%3 == 1){
            answer+="L";
            left.first = numbers[i]/3;
            left.second = 0;
        }
        //오른손 담당
        else if( numbers[i]!=0 && numbers[i]%3==0){
            answer+="R";
            right.first = numbers[i]/4;
            right.second = 2;
        }
        //중간 담당
        else
        {
            //거리 계산
            int x;
            int y;
            if(numbers[i] ==0)
                x =3,y=1;
            else
                x = numbers[i]/3,y = numbers[i]%3 -1;
            int leftdis =  abs(left.first-x) + abs(left.second-y);
            int rightdis =  abs(right.first-x) + abs(right.second-y);
            if(leftdis == rightdis){
                if(hand == "right")
                {
                    answer += "R";
                    right.first = x;
                    right.second =y;
                }
                else
                {
                    left.first = x;
                    left.second = y;
                    answer += "L";
                }
            }
            else{
                if(leftdis < rightdis){
                    answer+="L";
                    left.first = x;
                    left.second = y;
                }
                else
                {
                    answer +="R";
                    right.first = x;
                    right.second =y;
                }
            }
            
            
        }
    }
    
    return answer;
}
```

-----

## 5. 결과

필요시.














 
