---
title: Programmers_Greedy03 - 섬 연결하기
author: 강민석
date: 2021-02-01 13:09:00 +0800
categories: [Algorithm,Greedy]
tags: [Programmers,Greedy]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -섬  연결하기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Greedy03/Problem.JPG)  
![](/assets/img/sample/Programmers/Greedy03/Problem2.JPG)  

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 3의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 크루스칼 알고리즘 문제입니다.  

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
#include<queue>
#include<algorithm>
using namespace std;

//부모 노드를 가져옴
int getParent(int set[], int x)
{
	if (set[x] == x)return x;
	return set[x] = getParent(set, set[x]);
}

//부모 노드를 병합
void unionParent(int set[], int a, int b)
{
	a = getParent(set, a);
	b = getParent(set, b);
	if (a < b)set[b] = a;
	else set[a] = b;
}

int find(int set[], int a, int b)
{
	a = getParent(set, a);
	b = getParent(set, b);
	if (a == b)return 1;
	else
		return 0;
}

class Edge {
public:
	int node[2];
	int distance;
	Edge(int a, int b, int distance)
	{
		this->node[0] = a;
		this->node[1] = b;
		this->distance = distance;
	}
	bool operator<(Edge& edge)
	{
		return this->distance < edge.distance;
	}
};

int solution(int n, vector<vector<int>> costs) {
    int answer = 0;
	vector<Edge> v;
	for (size_t i = 0; i < costs.size(); ++i)
	{
		v.push_back(Edge(costs[i][0], costs[i][1], costs[i][2]));
	}
	sort(v.begin(), v.end());
	int* set = new int[n+1];
	for (int i = 0; i < n; ++i)
	{
		set[i] = i;
	}

	for (int i = 0; i < v.size(); ++i)
	{
		if (!find(set, v[i].node[0], v[i].node[1]))
		{
			answer += v[i].distance;
			unionParent(set, v[i].node[0], v[i].node[1]);
		}
	}

    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/Greedy03/result.JPG)  












 
