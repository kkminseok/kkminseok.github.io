---
title: Programmers_Heap03 - 이중우선순위 큐
author: 강민석
date: 2021-01-27 13:09:00 +0800
categories: [Algorithm,Heap]
tags: [Programmers,Heap]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 이중우선순위 큐 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/HEAP_03/Problem.JPG)  



-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 어떤 자료구조에 앞 뒤로 숫자를 넣고 빼는 것은 deque에 어울리지 않을까해서 deque를 사용했습니다. 
- 불필요한 반복을 줄이기 위해 정렬 여부를 판단하는 bool 타입 변수 check를 두었습니다.
- Insert만 들어올 경우를 대비해 마지막에 한 번 더 정렬을 해줬습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<deque>
#include<iostream>
#include<algorithm>
using namespace std;

bool pred(int& a, int& b)
{
    return a < b;
}

vector<int> solution(vector<string> operations) {
    vector<int> answer;
    deque<int> dq;
    string str;
    //정렬 판단 여부
    bool check =  false;
    for (size_t i = 0; i < operations.size(); ++i)
    {
        str = operations[i];
        // I,D 인지 확인
        char query = str[0];
        //숫자가 앞에 오도록 잘라줌
        str = str.substr(2);
        // 뒤의 숫자 문자열을 숫자 자료형으로 치환
        int insert = stoi(str);
        if (query == 'I')//insert
        {
            dq.push_back(insert);
            //정렬해라 라는 의미
            check = true;
        }
        else
        {
            if (insert == 1)
            {
                //정렬을 해야하면
                if (check)
                {
                    sort(dq.begin(), dq.end(),pred);
                    check = false;
                }
                if(!dq.empty())
                    dq.pop_back();
            }
            else
            {
                if (check)
                {
                    sort(dq.begin(), dq.end(),pred);
                    check = false;
                }
                if (!dq.empty())
                    dq.pop_front();
            }
        }
    }
    if (dq.empty())
    {
        answer.push_back(0);
        answer.push_back(0);
    }
    else
    {
        //Insert만 들어올 경우 정렬 다시 해줘야하므로
        sort(dq.begin(), dq.end(), pred);
        answer.push_back(dq.back());
        answer.push_back(dq.front());
    }

    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/HEAP_03/result.JPG)











 