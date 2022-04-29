---
title: Programmers_2021 KAKAO Intership 숫자 문자열과 영단어(python)
author: 강민석
date: 2021-07-19 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 숫자 문자열과 영단어 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/81301>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2021 kakao 카카오 채용연계형 인턴쉽 문제입니다.

Level 1난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문자열의 길이가 길지 않아서 아무렇게나 풀어도 됩니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(s):
    answer = ""
    dic = {}
    dic["zero"]=0
    dic["one"]=1
    dic["two"]=2
    dic["three"]=3
    dic["four"]=4
    dic["five"]=5
    dic["six"]=6
    dic["seven"]=7
    dic["eight"]=8
    dic["nine"]=9
    temp = ""
    for i in range(len(s)):
        if s[i].isalpha() : 
            temp+=s[i]
            if temp in dic : 
                answer += str(dic[temp])
                temp = ""
        else : 
            answer += str(s[i])
        
        
    return int(answer)
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
