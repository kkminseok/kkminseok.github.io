---
title: Programmers_2020 카카오 인턴십 수식 최대화(python)
author: 강민석
date: 2021-05-23 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -수식 최대화 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/67257#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2020 카카오 인턴십 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- brute하게 풀었습니다.
- oper라는 list에는 숫자를 담고, op라는 list에는 연산자를 담았습니다.
- 변수가 많아서 오타 등으로 인해 테스트케이스가 많이 실패했습니다.. for문도 깔끔하게 작성하지 못하여 보기 불편하실 수 있습니다.

-----  

## 4. 접근 방법을 적용한 코드


```python

import copy
def solution(expression):
    answer = 0
    oper = []
    op = []
    i = 0
    #temp는 string으로 들어온 expression의 숫자들을 임시로 저장하여 나중에 stoi를 해줍니다.
    temp = ""
    # expression을 돌면서 숫자면 temp에 저장하고 아니면 operlist에 temp를 넣고 초기화해줍니다.
    while i < len(expression):
        if(expression[i].isdigit()) : 
            temp+=expression[i]
        else:
            oper.append(int(temp))
            temp=""
            op.append(expression[i])
        i+=1
    # 마지막에 while문을 저장하지 않고 빠져나오므로 따로 또 저장해줍니다.
    oper.append(int(temp))

    def calcu(oper,op,idx):
        #idx : 0 -> +, 1 -> -, 2 -> *
        k = 0
        if idx ==0 :
            while k <len(op):
                if op[k] =="+":
                    first,second = oper[k], oper[k+1]
                    sum = first+second
                    oper[k] = sum
                    del oper[k+1]
                    del op[k]
                else:
                    k+=1
        elif idx==1 : 
            while k < len(op) :
                if op[k] =="-":
                    first,second = oper[k], oper[k+1]
                    sum = first-second
                    oper[k] = sum
                    del oper[k+1] 
                    del op[k]
                else:
                    k+=1
        else : 
            while k <len(op):
                if op[k] =="*":
                    first,second = oper[k], oper[k+1]
                    sum = first*second
                    oper[k] = sum
                    del oper[k+1]
                    del op[k]
                else :
                    k+=1
    # 여기서부터는 더 이쁘게 쓸 수 있을 것 같은데 아직 python문법에 미숙하여 직관적으로 썼습니다.
    # 0 1 2
    # 1 2 0
    # 2 0 1
    for idx1 in range(3):
        idx2 = (idx1+1) % 3
        idx3 = (idx1+2) % 3
        copyoper = copy.deepcopy(oper)
        copyop = copy.deepcopy(op)
        calcu(copyoper,copyop,idx1)
        calcu(copyoper,copyop,idx2)
        calcu(copyoper,copyop,idx3)
        answer = max(answer,abs(copyoper[0]))
    # 이것도 마찬가지..
    #  2 1 0
    #  1 0 2
    #  0 2 1
    for idx1 in range(2,-1,-1):
        idx2 = (idx1-1) 
        if idx2 < 0 :
            idx2 = 2
        idx3 = (idx1-2)
        if idx2 ==0 :
            idx3 = 2
        elif idx3 <0 :
            idx3 = 1
        copyoper = copy.deepcopy(oper)
        copyop = copy.deepcopy(op)
        calcu(copyoper,copyop,idx1)
        calcu(copyoper,copyop,idx2)
        calcu(copyoper,copyop,idx3)
        answer = max(answer,abs(copyoper[0]))
                
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
