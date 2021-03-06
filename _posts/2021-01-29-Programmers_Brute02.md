---
title: Programmers_BruteForce02 - 소수찾기
author: 강민석
date: 2021-01-29 13:09:00 +0800
categories: [Algorithm,Brute]
tags: [Programmers,Brute]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -소수찾기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Brute02/Problem.PNG)  
![](/assets/img/sample/Programmers/Brute02/Problem2.PNG)  

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 문자열.. 어질어질
- 문자열의 크기만큼 에라토스테네스의 체를 이용해 소수를 저장했습니다.
- 그 이후 문자열의 각각 인덱스의 정수값을 카운트하여 소수 전체를 돌며 속한 값이 없으면 0을리턴하고 하나라도 존재하면 1을 리턴하여 더했습니다. 



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<cmath>
#include<cstring>
using namespace std;
bool* PrimeArray;

int counting[10] = { 0, };
void Eratos(int n)
{
	PrimeArray = new bool[n + 1];
	cout << n;
	if (n <= 1) return;

	for (int i = 2; i <= n; i++)
		PrimeArray[i] = true;
	for (int i = 2; i * i <= n; i++)
	{
		if (PrimeArray[i])
			for (int j = i * i; j <= n; j += i)
				PrimeArray[j] = false;
	}

	// 이후의 작업 ...
}
bool check(int* counting,int input)
{
	while (input != 0)
	{
		int num1 = input % 10;
		if (counting[num1] == 0)
			return false;
		else
			counting[num1]--;
		input /= 10;
	}
	return true;
	
}

int solution(string numbers) {
	int answer = 0;
	size_t strsize = numbers.size();
	int tempcount[10] = { 0, };
	for (size_t i = 0; i < strsize; ++i)
	{
		counting[numbers[i] - 48]++;
	}
	int maxsize = pow(10, strsize);
	Eratos(maxsize);
	for (int i = 2; i < maxsize; ++i)
	{
		if (PrimeArray[i])
		{
			memcpy(tempcount, counting,sizeof(counting));
			answer += check(tempcount,i);
		}
	}
	return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Brute02/result.PNG)  












 
