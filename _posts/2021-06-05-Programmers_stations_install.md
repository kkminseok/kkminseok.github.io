---
title: Programmers_Summer/Winter Coding(~2018) 기지국 설치(python)
author: 강민석
date: 2021-06-05 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -기지국 설치 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12979#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Summer/Winter Coding(~2018) 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- brute하게 접근하면 당연히 시간초과가 뜰 것이라고 생각했습니다.(N이 최대 2억까지 들어옴.)
- 결국 최소한으로 기지국을 설치하려면 최대한 넓은 영역에 신호가 미치도록 설치해야하는 것은 맞습니다. 이 부분에서 설치할 기지국은 (남은 영역) / (전파가 영향을 끼치는 영역)으로 구할 수 있습니다.
- 그 영역들은 이전 stations의 +w 그 다음 stations의 -w의 차이가 남은 영역입니다.
    + 예제 1에서 stations가[4,11] 으로 (4+1) 과 (11-1)인 5 부터 10까지가 빈 영역입니다. 
    + 조심해야할 것은 10-5해서 5가 빈 영역이 아닌 10-5-1인 4가 빈 영역입니다. (범위에 10과 5가 둘 다 포함되어 있으므로)
- 마찬가지로 가장 처음 기지국을 설치할 영역과 마지막에 기지국을 설치할 영역을 따로 구해주면 풀 수 있습니다.(배열안에 마지막(N)과 처음(1)이 없으므로)








-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(n, stations, w):
    answer = 0
    #기지국이 전파를 미치는 영향 예제 1 기준 3 / 2 기준 5
    area = w*2 +1
    #처음
    check = stations[0] - w 
    # 만약 뺀 값이 범위를 벗어나면 skip을 하고 아니면 계산해줍니다.
    if check >1 : 
        #영역이 1부터 시작하므로 check-1을 해줍니다.
        answer += (check -1 ) // area
        # 나머지가 있다면 어찌되었든 기지국 1개를 설치해야합니다.
        if (check-1) %area != 0 :
            answer+=1
    #기지국 정보에 따른 값을 수정합니다.
    for i in range(len(stations)-1):   
            #빈 영역 계산 1을 빼주는 이유는 설명에서 말했습니다.
            check = (stations[i+1]-w) - (stations[i]+w)-1
            #인접하거나 겹치는 경우 skip합니다.
            if check <= 0 : 
                continue
            else : 
                answer += (check) // area
                if check %area!=0:
                    answer+=1
    #끝 부분            
    check = stations[len(stations)-1] + w
    #끝의 기지국의 영향이 n을 넘어가면 skip합니다.
    if check <n : 
        answer += (n - check) // area
        if (n-check) % area != 0 :
            answer +=1
            
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
