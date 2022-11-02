---
title: Programmers_옹알이(1)(python)
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-10-28 00:00:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
  path: 
comments : true
---

> 이 글을 보시기 전에 문제를 풀기 위해 충분한 생각을 하셨나요? 답을 안 보고 푸는게 최대한 고민하는게 가장 중요하다고 생각합니다.!!
{: .prompt-warning}


**프로그래머스 옹알이(1) 문제 입니다.**

## 1.☑️ 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/120956?language=python3>

-----  

## 2.☑️ 분류 및 난이도

Programmers 문제입니다.  

Level 1난이도의 문제입니다.


-----  

## 3.☑️ 생각한 것들(문제 접근 방법)

- 은근 까다롭지만 브루트하게 풀면 풀 수 있습니다.


-----  

## 4.☑️ 접근 방법을 적용한 코드

```python
def solution(babblings):
    answer = 0

    for babbling in babblings:
        idx = 0
        size = len(babbling)
        while idx < size:
            if babbling[idx] == 'a' and idx + 2 < size and babbling[idx:idx+3] == 'aya':
                idx +=3
            elif babbling[idx] == 'y' and idx + 1 < size and babbling[idx:idx+2] == 'ye':
                idx +=2
            elif babbling[idx] == 'w' and idx + 2 < size and babbling[idx:idx+3] == 'woo':
                idx +=3
            elif babbling[idx] == 'm' and idx + 1 < size and babbling[idx:idx+2] == 'ma':
                idx +=2
            else:
                break
        if idx == size:
            answer += 1
               
    return answer
```


-----

브루트하게 조건을 모두 적어줘서 풀었습니다.



## 5.☑️ 결과

> c++로 작성이 필요하거나 도움이 필요하시면 댓글을 작성해주세요.!! 기록용이라 설명이 자세하지 않습니다.
{: .prompt-info }