---
title: Programmers_Greedy01 - 체육복
author: 강민석
date: 2021-02-01 13:03:00 +0800
categories: [Algorithm,Greedy]
tags: [Programmers,Greedy]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 체육복 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Greedy01/Problem.JPG)  
![](/assets/img/sample/Programmers/Greedy01/Problem2.JPG)  

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 1의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 학생수 보다 조금 넉넉하게 배열을 만들어 관리했습니다.
- 잃어버린 학생한테는 --를 여유분이 있는 학생한테는 ++를 해주었습니다.
- 배열을 검사하며 뒤에있는 사람이 체육복을 주는 형태로 작성하였는데, 생각해보니 앞에있는 사람이 체육복을 주는 케이스가 있었습니다.
```console
8, [2,3,4] [1]인경우
```
-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
using namespace std;

int solution(int n, vector<int> lost, vector<int> reserve) {
    int answer = 0;
    int* stu = new int[n+2];
    for (int i = 0; i < n+1; ++i)
    {
        stu[i+1] = 1;
    }

    for (size_t i = 0; i < reserve.size(); ++i)
    {
        stu[reserve[i]]++;
    }
    for (size_t i = 0; i < lost.size(); ++i)
    {
        stu[lost[i]]--;
    }
    for (int i = 1; i < n+1; ++i)
    {
        if (stu[i]==0)
        {
            if (i + 1 < n + 1 && stu[i + 1] > 1)
            {
                stu[i]++;
                stu[i + 1]--;
            }
            else if (i - 1 > 0 && stu[i - 1] > 1)
            {
                stu[i]++;
                stu[i - 1]--;
            }
        }
        if (stu[i] > 0)
            ++answer;
        cout << stu[i] << " ";
    }



    return answer;
}
```
-----

## 5. 결과

  
![](/assets/img/sample/Programmers/Greedy01/result.JPG)  













 
