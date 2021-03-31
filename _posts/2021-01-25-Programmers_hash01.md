---
title: Programmers_hash01 - 완주하지 못한 선수
author: 강민석
date: 2021-01-25 09:02:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,HASH]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -해시 - 완주하지 못한 선수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/hash_01/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.
해시 중에 가장 쉬운 Level1 문제입니다.  


-----  

## 3. 생각한 것들(문제 접근 방법)

- 대놓고 hash 라고 써 있어서 c++ STL을 이용하려 했지만, 표준이 아닌 hash STL은 사용할 수 없었습니다.
- vector의 인풋이 최대 10만개 들어오고 비교대상도 99,999개라고 했으니 일반적으로 값들을 하나하나 찾아주면 효율성의 문제가 생길거라 생각했습니다.
- **다른 한사람**만 찾으면 되기에 정렬을 하고 틀린 배열이 나온경우 결과를 리턴하는 식으로 작성하였습니다.
- 최악의 경우 맨 마지막 배열에서 다른 사람이 존재할 경우 배열비교 시 인덱스 범위를 벗어나므로 예외처리를 하나 해줬습니다.  



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include<iostream>
#include <string>
#include <vector>
#include<algorithm>
using namespace std;

string solution(vector<string> participant, vector<string> completion) {
    int counting =0;
    string answer = "";
    sort(participant.begin(),participant.end());
    sort(completion.begin(),completion.end());
    for (int size = 0;size<participant.size();++size)
    {
        //만약 비교대상의 인덱스를 벗어날 경우 case1인경우
        if(size>completion.size())
        {
            answer = participant[size];
            return answer;
        }
        if(participant[size]!=completion[size])
        {
            answer = participant[size];
            return answer;
        }
    }
    return answer;
}
```
-----

## 5. 결과


![](/assets/img/sample/Programmers/hash_01/result.JPG)











 