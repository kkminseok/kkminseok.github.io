---
title: Programmers_hash03 - 위장
author: 강민석
date: 2021-01-25 12:05:00 +0800
categories: [Algorithm,HASH]
tags: [Programmers,HASH]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -해시 - 위장 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/hash_03/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.
Level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 인풋데이터는 적은 편이라 수행시간은 고려안했습니다.
- 다른 문제와 같이 정렬을 이용한 꼼수로 풀려했는데, 잘 되지 않아 STL의 map을 이용했습니다.
     + 이 부분에서 고민이 많았던게, 이미 vector에 담겨진 값들을 다시 map에 옮기는 것이 효율성을 저하한다고 생각하여 안쓰려고 했으나 쉽지 않았습니다.
- 처음에 틀렸었는데, 이유는 경우의 수를 세주는 부분에서 틀렸습니다.
- 인풋에서 옷의 종류가 중요하지 옷의 이름은 중요하지 않습니다. (값 도출에 쓰이지 않는다.)
     + 옷을 무조건 1개 씩 입는 가지 수 + **옷을 입었을 때의 경우의 수** 에서 틀렸습니다.
     + 어떠한 옷에 대해서 안입어도 되는 것인데 다 입을 것이라고 가정하고 경우의 수를 셌습니다. 예를 들어 
     바지 2개 상의 1 겉옷 1개면 경우의 수는  2 * 1 * 1 로 도출했는데, 옷을 안입는 경우를 하나씩 더해주고 마지막에 옷을 하나도 안입는 경우의 수를 빼주면 됩니다.
     (2+1) * (1+1) * (1+1) -1(전부 안 입는 경우) 뒤의 1들은 옷을 안 입는 경우입니다.
     
- 처음에 틀린 코드 입니다.

```c++
#include <string>
#include <vector>
#include<iostream>
#include<algorithm>
#include<map>
using namespace std;

int solution(vector<vector<string>> clothes) {
    int answer = 0;
    //1가지 씩 입는 경우
    answer+=clothes.size();
    //문자열과 카운트 값
    map<string,int> m;
    for(size_t i=0;i<clothes.size();++i)
    {
        if(m.find(clothes[i][1])==m.end())
        {
            m.insert(make_pair(clothes[i][1],1));
        }
        else
            m[clothes[i][1]]++;
    }
    if(m.size()>1)
    {
        int counting = 1;
        map<string,int>::iterator iter;
        for(iter = m.begin(); iter!=m.end();++iter)
        {
            counting*=(*iter).second;
        }
        answer+=counting;
    }    
    return answer;
}
```




-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<algorithm>
#include<map>
using namespace std;

int solution(vector<vector<string>> clothes) {
    int answer = 0;
    //문자열과 카운트 값
    map<string,int> m;
    for(size_t i=0;i<clothes.size();++i)
    {
        if(m.find(clothes[i][1])==m.end())
        {
            m.insert(make_pair(clothes[i][1],1));
        }
        else
            m[clothes[i][1]]++;
    }
    if(m.size()>1)
    {
        int counting = 1;
        map<string,int>::iterator iter;
        for(iter = m.begin(); iter!=m.end();++iter)
        {
            counting*=((*iter).second+1);
        }
        answer+=counting;
        answer-=1;
    }
    else
    {
        answer +=clothes.size();
    }
    return answer;
}
```
-----

## 5. 결과


![](/assets/img/sample/Programmers/hash_03/result.JPG)











 