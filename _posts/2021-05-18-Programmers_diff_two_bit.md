---
title: Programmers_2개 이하로 다른 비트(python)
author: 강민석
date: 2021-05-18 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -2개 이하로 다른 비트 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/77885#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌2 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 규칙을 찾아야하는 문제입니다.
- 질문하기에서 힌트를 얻어 작성하였습니다.
- '11011' 이라는 숫자가 있으면 0이 처음으로 나오는 곳을 찾아 2진수로 만듭니다. -> '00100' 그리고 그 보다 하나 오른쪽으로 쉬프트한 값 '00010'의 차이만큼 더해주면 값이 나옵니다.





-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(numbers):
    answer = []
    
    for i in range(len(numbers)):
        num = numbers[i]
        cnt = 0
        while 1:            
            if (num & 1 <<cnt) ==0 :
                break
            cnt+=1
        if cnt != 0 :
            num = num + ((1<<cnt)) - (1<<(cnt-1))
        #끝자리가 0인 경우 1만 더하면 된다.
        else : 
            num += 1
        answer.append(num)
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
