---
title: Programmers_2018 KAKAO BLIND RECRUITMENT [1차] 추석 트래픽(python)
author: 강민석
date: 2021-06-14 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -추석 트래픽 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17676>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
KAKAO BLIND RECRUITMENT(2018) - [1차] 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 처음 풀어본 KAKAO LEVEL3문제입니다.
- python의 datetime 라이브러리를 사용하였습니다.
- 구간 설정 하는데에 시간을 많이 썼는데 결과적으로 모든 line을 돌면서 line마다의 끝 시간  + 1초 구간을 정해 카운트를 세줘야합니다. 
- 예제 2에서 힌트를 얻었는데, 왜 인지는 모르겠습니다.
- 문제에서 1초는 **시작 시간과 끝 시간을 포함**합니다.
- 즉 5초의 1초는 5:000s ~ 5.999s입니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
# 자료구조 꺼내를 빠르게 하기 위함.
from collections import deque
# datetime은 시간라이브러리, timedelta는 시간 연산을 위한 라이브러리 같습니다.
from datetime import datetime,timedelta
def solution(lines):
    answer = 0
    dq = []
    # compare은 가장 빨리 끝나는 line을 저장합니다.
    compare = datetime(2018,1,1,1,1,1)
    # last는 가장 늦게 끝나는 line을 저장합니다.
    last = datetime(2014,1,1,1,1,1)
    # line을 돌면서 시간에 대해 전처리를 해준 뒤 리스트에 넣습니다.
    for i in range(len(lines)):
        # "-"로 먼저 구분합니다.
        year,mon,day = lines[i].split("-")
        day,mod,query = day.split(" ")
        # 끝의 s를 제거
        query = query[:-1]
        querys = query.split(".")
        queryms =  0
        # 2.0s와 2s를 동일하게 여겨야합니다. 또한, 2.321s같이 들어올 경우 마이크로 초를 구분해줘야합니다.
        # 연산이 이렇게 되는 이유는 기본적으로 timedate에서 제공하는 microseconds의 범위는 0 ~ 100000입니다. 문제에서는 소수점 3자리까지만 나타내므로 전처리를 해줘야합니다.
        if len(querys) > 1 : 
            queryms = int(querys[1]) * 1000000  //(10**len(querys[1]))
        querys = int(querys[0])
        h,m,s = mod.split(":")
        s,ss = s.split(".")
        # end는 line으로 들어온 값입니다.
        end = datetime(int(year),int(mon),int(day),int(h),int(m),int(s),int(ss)*1000)
        # 제일 늦게끝난 것은 last에 빨리 끝난 것은 compare에 저장합니다.
        if end > last : 
            last = end
        if compare > end : 
            compare =end
        # start는 위에서 전처리해준 T값을 빼준 시간입니다.
        start = end - timedelta(seconds = querys, microseconds = queryms) + timedelta(microseconds=1000)
        # 리스트에 넣습니다.
        dq.append((start,end))
    # 리스트를 돌 것인데, 정렬되어있지 않으므로 전부 두 번씩 돌아줍니다.
    for s,e in dq :
        count = 0
        compare = e
        for s,e in dq : 
            # 만약 범위를 벗어났으면 continue합니다.
            if compare > e or  compare + timedelta(microseconds = 999000) < s:
                continue
            else:
                count+=1
                
        answer = max(answer,count)
                
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
