---
title: Programmers_쿼드압축 후 개수 세기
author: 강민석
date: 2021-05-07 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -쿼드압축 후 개수 세기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/68936>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌 1 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 쿼드 압축은 대표적인 재귀문제 중 하나로 방식을 이해하고 있으면 어렵지 않게 해결할 수 있습니다.
- 저는 방식을 잘 몰라서 참고하였습니다.





-----  

## 4. 접근 방법을 적용한 코드


```python
one = 0
zero = 0
def solve(curX, curY,length,arr):
    global zero,one
    check = True

    ch = arr[curX][curY]
    
    if length == 1 : 
        check = True
    else : 
        for i in range(length):
            for j in range(length):
                if ch != arr[curX+i][curY+j] : 
                    check = False
                    break
                if not check : 
                    break
    if check : 
        if ch == 0 : 
            zero += 1
        else : 
            one +=1
    else : 
        templ = int(length/2)
        solve(curX,curY,templ,arr)
        solve(curX,curY + templ,templ, arr)
        solve(curX + templ, curY ,templ,arr)
        solve(curX + templ, curY + templ ,templ,arr)

def solution(arr):
    answer = []
    solve(0,0,len(arr),arr)
    answer.append(zero)
    answer.append(one)
    return answer
```


-----



## 5. 결과

필요시. python로 짜드리겠습니다.















 
