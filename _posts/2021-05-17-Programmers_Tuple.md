---
title: Programmers_튜플
author: 강민석
date: 2021-05-17 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -튜플 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/64065>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2019 KaKao 개발자 겨울 인턴십 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문자열로 들어오고, 입력이 백만자까지, 튜플의 크기는 최대 10만, 튜플 속의 원소의 크기는 최대 500 이기에, 하나의 for문으로 해결해야 한다고 생각했습니다.
- 문자열을 돌면서 정수값을 기준으로 dictionary에 값을 넣으면서 카운트를 세주고, 가장 많이 세진 정수값을 기준으로 정렬을 해줍니다.
- 정렬된 값을 순서대로 answer 리스트에 넣어주면 해결할 수 있습니다.




-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(s):
    answer = []
    dic ={}
    # 문자열을 돌기위한 index(idx)
    idx = 0 
    while idx < len(s) : 
        # temp에 숫자들을 저장해줍니다.
        temp = ''
        while s[idx].isdigit():
            temp += s[idx]
            idx +=1
        # 만약 temp에 숫자가 들어갔으면 숫자로 바꿔주고 dictionary에 넣어줍니다.
        if temp!='' : 
            temp = int(temp)
            if temp not in dic :
                dic[temp] = 1
            else:
                dic[temp] +=1
        # 숫자가 들어가지 않았으면, 
        else:
            idx+=1
    # 역순으로 정렬해줍니다.
    res = sorted(dic.items(),key = (lambda x:x[1]),reverse = True)
    for i in range(len(res)):
        answer.append(res[i][0])
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
