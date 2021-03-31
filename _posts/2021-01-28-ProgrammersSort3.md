---
title: Programmers_sort03 - H-index
author: 강민석
date: 2021-01-28 15:09:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Sort]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -H-index 수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Sort3/Problem.PNG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 헷갈려서 생각하는 데 시간이 좀 걸렸습니다.
- 인덱스 접근만 잘 하면 풀 수 있는 문제 같았습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<algorithm>
using namespace std;

int solution(vector<int> citations) {
	int answer = 0;
	sort(citations.begin(), citations.end(),greater<int>());
	if (citations[0] == 0)
		return 0;
	for (size_t i = 0; i < citations.size(); ++i)
	{
		if (citations[i] > i)
			++answer;
	}

	return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Sort3/result.PNG)  












 
