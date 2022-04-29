---
title: Programmers_땅따먹기
author: 강민석
date: 2021-05-06 05:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -땅따먹기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12913>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
연습문제 입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- DP로 풀 수 있나 생각하다가, Greedy로도 풀 수 있을 것 같아서 Greedy로 시도했습니다.
- 효율성 부분에서 매우 안좋은 평가(?)가 나온 것 같지만, 풀었으니 제출하겠습니다.




-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(land):
    answer = 0
    for i in range(1,len(land)):
        for j in range(4):
            maxidx = 0
            for k in range(4):
                if k==j :
                    continue
                maxidx = max(maxidx,land[i-1][k])
            land[i][j] = land[i][j] + maxidx
            answer = max(answer,land[i][j])
    return answer
```


-----



## 5. 결과

필요시. c++로 짜드리겠습니다.















 
