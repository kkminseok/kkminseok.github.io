---
title: Programmers_Summer/Winter Coding(~2018) 숫자 게임(python)
author: 강민석
date: 2021-06-05 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -숫자 게임 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/12987>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Summer/Winter Coding(~2018) 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- B입장에서 가장 기분 좋은 승리는 무엇일까?를 생각해봤습니다.
- B입장에서 가장 기분 좋게 승리하는 방법은 A낸 카드에서 제일 근소한 차이를 내며 이기는 방법이 좋은 승리라고 생각했습니다.
- 예를 들어서 A가 [1,3,5,7]를 들고 있고 B가 [8,6,4,2]를 들고 있으면 [2,4,6,8]로 가장 근소한 차이로 승점을 4개를 챙길 수 있습니다.
- 이러한 방식을 대입하려면 A와 B를 정렬해야한다고 생각했습니다.(문제에서 어차피 A의 패를 알고 B는 그에 맞춰 내기 때문에 순서는 상관없음.)
- 근데 만약 [1,3,5,7]를 들고 있는데, B가 [2,2,6,8] 이렇게 들고 있으면 어떻게해야할까? 
- 간단합니다. 문제가 되는 두 번째 [2] 카드를 그냥 버리고 6을 내면 됩니다.
- 앞에서 지나 뒤에서 지나 똑같기 때문입니다.




-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(A, B):
    answer = 0
    A.sort()
    B.sort()
    Aidx = 0
    Bidx = 0 
    #A의 끝을 보면 A를 다 이긴 것이고,  B의 끝을 보면 B에서 더 이상 이길 수 있는 카드가 없다는 것입니다.
    while Aidx < len(A) and Bidx < len(B) :
        if A[Aidx] <B[Bidx] : 
            answer+=1
            Aidx +=1
            Bidx+=1
        #이기는 카드를 찾을 때 까지.
        else : 
            Bidx+=1
            
        
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
