---
title: Programmers_Greedy04 - 큰 수 만들기
author: 강민석
date: 2021-02-02 13:03:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Greedy]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 큰 수 만들기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Greedy04/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 처음에는 조합을 생각했습니다.
- 크기가 백만이라 일반적인 조합은 안되고 DP를 이용해서 풀어야할텐데, 방법을 찾지 못해서 당연히 틀렸습니다.  


**틀린 코드**
```c++
#include <string>
#include <vector>
#include<iostream>
using namespace std;
string result = "";

void Combination(string number, string temp, int size, int index, int depth)
{
    if (temp.size() == depth)
    {
        result = result < temp ? temp : result;
        cout << result << '\n';
    }
    else
    {
        for (int i = index; i < number.size(); ++i)
        {
            temp[depth] = number[i];
            Combination(number, temp, size, i + 1, depth + 1);

        }
    }
}

string solution(string number, int k) {
    string answer = "";
    int size = number.size() - k;
    string temp;
    temp.assign(size,'A');

    Combination(number, temp, size, 0, 0);
    answer = result;
    return answer;
}
```

- DP로 풀겠다는 집념으로 여러가지 찾아봤으나 힘들 것 같아서 포기하고 타 블로그의 도움을 받았습니다.  
<https://mtoc.tistory.com/80>

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
using namespace std;

string solution(string number, int k) {
    string answer = "";
    int size = number.size() - k;
    
    int start = 0;
    for (int i = 0; i < size; ++i)
    {
        char MAXnum = number[start];
        int MaxIdx = start;
        for (int j = start; j <= i + k; ++j)
        {
            if (MAXnum < number[j])
            {
                MAXnum = number[j];
                MaxIdx = j;
            }
        }
        start = MaxIdx + 1;
        answer += MAXnum;
    }


    
    return answer;
}
```
-----

## 5. 결과
생각 한사람이 너무 대단하다고 느꼈습니다.  














 
