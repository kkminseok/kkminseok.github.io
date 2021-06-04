---
title: Programmers_월간 코드 챌린지 시즌1 풍선 터트리기(python)
author: 강민석
date: 2021-06-04 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -풍선 터트리기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/68646>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌1 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- a의 길이가 최대 백만이 들어오므로 brute하게 풀면 시간초과가 뜹니다.
- 기준 값을 중심으로 한쪽에 있는 배열중 가장 작은 값이 기준값보다 크면 다른 한쪽에 있는 배열중 가장 작은 값은 기준값보다 커야합니다.
- 이유는 토너먼트 방식으로 기준값의 왼쪽 배열, 오른쪽 배열 풍선 1개씩 남겨둘 수 있는데, 그 중 하나가 기준 값보다 작으면 문제에서 나온 대로 작은 값을 없애버릴 수 있지만 만약 둘 다 작은 경우(기준값이 두 값보다 큰 경우) 풍선을 터트릴 수 없게됩니다.
- 매 번 왼쪽 배열의 최소값, 오른쪽 배열의 최소값을 직접 구하면 시간초과가 뜨기에 오른쪽 배열의 최소값들은 따로 리스트에 저장해두었습니다.
- 위와 같은 방식으로 맨앞 경우 오른쪽 배열에서 가장 작은 값, 맨 뒤 경우 왼쪽 배열에서 가장 작은값만 남기는데, 그 값이 기준 값보다 커도 상관 없고, 작아도 상관없기에(작은 값을 지워버림.) a의 길이가 2보다 크거나 같다면 기본 return값은 2가 됩니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(a):
    answer = 1
    #a의 길이가 1인 경우
    if len(a) == 1:
        return 1
    #아닌 경우 default는 2
    answer+=1
    #right는 기준 값을 기준으로 오른쪽 배열에서 가장 작은 값들을 저장해놓은 리스트입니다.
    right = [0]* len(a)
    #초기값 설정
    rightmin = a[len(a)-1]
    #리스트를 돌면서 초기화
    for i in range(len(a)-2,0,-1) : 
        right[i] = rightmin
        if rightmin > a[i]:
            rightmin = a[i]
    #왼쪽배열중 가장 작은 값 초기화
    leftmin = a[0]
    #맨 왼쪽과 맨 오른쪽은 제외
    for i in range(1,len(a)-1) : 
        #만약 맨왼쪽의 최소값보다 작은 값이 나타난 경우 갱신
        if leftmin > a[i-1] : 
            leftmin = a[i-1]
        # 기준값이 최소값들보다 큰 경우 skip
        if leftmin < a[i] and right[i] < a[i] : 
            continue
        answer+=1
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
