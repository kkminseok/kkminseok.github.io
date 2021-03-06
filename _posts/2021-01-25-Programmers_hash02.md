---
title: Programmers_hash02 - 전화번호 목록
author: 강민석
date: 2021-01-25 11:02:00 +0800
categories: [Algorithm,HASH]
tags: [Programmers,HASH]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -해시 - 전화번호 목록 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/hash_02/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.
Level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 인풋 데이터가 백만으로 큽니다. 이것 또한 순차적으로 비교하면 시간초과가 날 것이라 생각했습니다.
- 앞 문제와 마찬가지로 정렬을 해주면 같은 것끼리 묶이면서 문자열이 좀 더 긴 것은 뒤로 가지 않을까 하면서 정렬을 해주었습니다.
- 정렬을 하면 문자열 비교는 최악의 경우 백 만정도 걸리게 됩니다. 
- 문제는 **단 한번이라도 접두사를 가진 문자열이 있느냐?**라는 초점에 따라 단 한번이라도 접두사를 가진 문자열을 찾으면 바로 결과를 리턴하도록 했습니다. 만약 한번도 거치지 않았다면 자동으로 true를 출력하게끔 하였습니다.




-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
#include<iostream>

using namespace std;

bool compare(string a,string b, int size)
{
    for(int i=0;i<size;++i)
    {
        //하나라도 다른 부분이 있다면
        if(a[i]!=b[i])
        {
            return true;
        }
    }
    return false;
}

bool solution(vector<string> phone_book) {
    bool answer = true;
    sort(phone_book.begin(),phone_book.end());
    for(size_t i = 0; i<phone_book.size()-1;++i)
    {
        int sizet = min(phone_book[i].size(),phone_book[i+1].size());
        answer = compare(phone_book[i],phone_book[i+1],sizet);
        if(!answer)
        {
            return answer;
        }
    }
    return answer;
}
```
-----

## 5. 결과


![](/assets/img/sample/Programmers/hash_02/result.JPG)











 