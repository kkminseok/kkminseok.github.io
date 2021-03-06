---
title: Programmers_BruteForce03 - 카펫
author: 강민석
date: 2021-01-29 15:09:00 +0800
categories: [Algorithm,Brute]
tags: [Programmers,Brute]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -카펫 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Brute03/Problem.PNG)  
![](/assets/img/sample/Programmers/Brute03/Problem2.PNG)  

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제 자체는 어렵지 않았으나, 왜인지 모르게 Testcase1에서 계속 틀림....
  + 사람들이 많이 풀지도 않았고, 나와 공통된 질문도 없어서 소인수분해로 문제를 풀기 시작했습니다.
  + 소인수 분해를 하면서 다른사람들의 질문을 보니 점화식이 존재.. brown = (yellow의 가로 + yellow의 세로 +2) * 2 이라는 식이 있었음..
  + 그래서 그걸 이용해서 코드를 짰습니다.

**틀린 코드**
```c++
#include <string>
#include <vector>
#include<iostream>

using namespace std;

bool checking(int tempbrown, int tempyellow, int brown, int yellow)
{
	for (int i = yellow; i > 0; --i)
	{
		if (yellow%i == 0)
		{
			if (tempbrown > i && tempyellow - 1 > (yellow / i))
			{
				cout << tempbrown << " " << tempyellow << " " << i << " " << yellow << '\n';
				return true;
			}
		}
	}
	return false;
}

vector<int> solution(int brown, int yellow) {
	vector<int> answer;
	int sum = brown + yellow;
	for (int i = sum; i >= 1; --i)
	{
		if (sum%i == 0)
		{
			if (i <(sum / i))
				return answer;
			int tempyellow = sum / i;
			if (tempyellow != yellow)
			{
				bool check = checking(i, tempyellow, brown, yellow);
				if (check)
				{
					answer.push_back(i);
					answer.push_back(tempyellow);
					return answer;
				}
			}
		}
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

using namespace std;

bool checking(int tempbrown, int tempyellow, int brown, int yellow)
{
	for (int i = yellow; i > 0; --i)
	{
		if (yellow%i == 0)
		{
			if (tempbrown > i && tempyellow - 1 > (yellow / i))
			{
				cout << tempbrown << " " << tempyellow << " " << i << " " << yellow << '\n';
				return true;
			}
		}
	}
	return false;
}

vector<int> solution(int brown, int yellow) {
	vector<int> answer;
	vector<int> sosu;
	int sum = brown + yellow;
	sosu.push_back(1);
	for (int i = 2; i <= yellow / 2; ++i)
	{
		if (yellow%i == 0)
		{
			sosu.push_back(i);
			cout << i<<" ";
		}
	}

	for (size_t i =0 ; i<sosu.size();++i)
	{
		int div = yellow / sosu[i];
		int result = (div + sosu[i] + 2) * 2;
		if (result == brown)
		{
			answer.push_back(div + 2);
			answer.push_back(sosu[i] + 2);
			break;
		}
	}
	return answer;
}
```
-----

## 5. 결과
테케1 틀린거에서 1시간 가까이 쓴 것 같습니다.

![](/assets/img/sample/Programmers/Brute03/result.PNG)  












 
