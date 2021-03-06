---
title: Programmers_sort01 - K번째 수
author: 강민석
date: 2021-01-28 15:09:00 +0800
categories: [Algorithm,Sort]
tags: [Programmers,Sort]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - K번째 수 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Sort01/Problem.PNG)  
![](/assets/img/sample/Programmers/Sort01/Problem2.PNG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 1의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 일이 많아서 빨리 끝내야한다는 생각에 비효율적인 코드를 마구 넣었습니다. 추천할만한 코드는 아닙니다.
- 반복문마다 객체 생성을 반복하면서 초기화도 반복!!... 굉장히 비효율적인 코드.
- 나머지는 인덱스 주어진만큼 정렬한 뒤, 정렬을 시작한 위치에서 주어진 K값을 더해 반환하였습니다.
-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <algorithm>
#include<iostream>
using namespace std;

vector<int> solution(vector<int> array, vector<vector<int>> commands) {
	vector<int> answer;

	for (size_t i = 0; i < commands.size(); ++i)
	{
		int* temp = new int[array.size()];
		for (size_t i = 0; i < array.size(); ++i)
		{
			temp[i] = array[i];
		}
		
		int start = commands[i][0]-1;
		int end = commands[i][1];
		int index = commands[i][2]-1;
        
		sort(temp+start, temp+end);
		answer.push_back(temp[index+start]);
		delete temp; 
	}

	
		return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Sort01/result.PNG)  












 
