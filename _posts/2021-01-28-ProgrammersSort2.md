---
title: Programmers_sort02 - 가장 큰 수
author: 강민석
date: 2021-01-28 15:09:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Sort]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 가장 큰 수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Sort01/Problem.PNG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 생각 자체는 어렵지 않았습니다. 배열을 돌면서 주어진 정렬 조건대로 answer에 추가해주었습니다.
- 인풋으로 0이 들어올 경우를 예외처리 해주었습니다.
-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<algorithm>
using namespace std;

bool pred(string& a, string& b)
{
	return a + b > b + a;
}


string solution(vector<int> numbers) {
	string answer = "";
	vector<string> vec;
	for (size_t i = 0; i < numbers.size(); ++i)
	{
		vec.push_back(to_string(numbers[i]));	
	}
	sort(vec.begin(), vec.end(),pred);
	for (size_t i = 0; i < numbers.size(); ++i)
		answer += vec[i];

    if(answer[0]=='0')
        answer='0';
	return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Sort01/result.PNG)  












 
