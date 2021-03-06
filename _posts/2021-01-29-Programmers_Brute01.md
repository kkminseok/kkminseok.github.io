---
title: Programmers_BruteForce01 - 모의고사
author: 강민석
date: 2021-01-29 12:09:00 +0800
categories: [Algorithm,Brute]
tags: [Programmers,Brute]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -모의고사 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Brute01/Problem.PNG)  
![](/assets/img/sample/Programmers/Brute01/Problem2.PNG)  

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 1의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 어렵지는 않지만, 효율성을 따지기 위해 이것저것 시도한 것 같습니다.
  + 하나의 for문에 담으려고 하였으나 더 복잡해질 것 같아서 배열 3개로 만들어서 비교해서 넣었습니다.
- 모두 0문제 맞췄을 때에 대한 예외처리는 없는 것 같습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<algorithm>
using namespace std;
const int MAX = 10001;

int arr[MAX] = { 1,2,3, };
int arr2[MAX] = { 0, };
int arr3[MAX] = { 0, };
int temp[5] = { 3,1,2,4,5 };
int temp2[4] = { 1,3,4,5 };	

bool pred(pair<int, int>& a, pair<int, int>& b)
{
  if (a.second == b.second)
    return a.first < b.first;
  else
    return a.second > b.second;
}

vector<int> solution(vector<int> answers) {
  vector<int> answer;

  int init = 0;
  int init2 = 0;
  int init3 = 0;
  vector<pair<int, int>> vec;
  vec.push_back(make_pair(1, 0));
  vec.push_back(make_pair(2, 0));
  vec.push_back(make_pair(3, 0));

  for (int i = 0; i < MAX; ++i)
  {
    arr[i] = init % 5+ 1;
    ++init;
    if (i % 2 == 0)
    {
      arr2[i] = 2;
    }
    else
    {
      arr2[i] = temp2[init2];
      init2++;
      if (init2 > 3)
        init2 = 0;
    }
    arr3[i] = temp[init3];
    if (i % 2 == 1)
    {
      init3++;
      if (init3 > 4)
        init3 = 0;
    }
  }
  for (size_t i = 0; i < answers.size(); ++i)
  {
    if (answers[i] == arr[i])
      vec[0].second++;
    if (answers[i] == arr2[i])
      vec[1].second++;
    if (answers[i] == arr3[i])
      vec[2].second++;
  }
  sort(vec.begin(), vec.end(),pred);
  if (!(vec[0].second == 0))
  {
    for (size_t i = 0; i < vec.size(); ++i)
    {
      answer.push_back(vec[i].first);
      if (i + 1 < vec.size() && vec[i].second == vec[i + 1].second)
        continue;
      else
        break;
    }
  }
  return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Brute01/result.PNG)  












 
