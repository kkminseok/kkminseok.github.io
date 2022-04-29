---
title: Programmers_멀쩡한 사각형
author: 강민석
date: 2021-05-16 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -멀쩡한 사각형 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/62048>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Summer / Winter Coding(2019) 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제는 직관적으로 어렵지 않습니다.
- 입력이 최대 1억씩 들어오므로 DP로 푸는 것은 불가능 하다고 생각했습니다.
- 따라서 정해진 공식이 있을 것인데, 문제를 보면 2,3를 기준으로 꼭지점을 지납니다.
- 따라서 2와 3의 배수인 8,12 는 완벽하게 꼭지점을 통과하게됩니다.(4,6) , (6,9) 를 거쳐서
- 2,3를 기준으로 4개의 블럭을 못 쓰는데, 이는 w + h - 1 공식에 따라 4개로 지정됩니다. 이에 대한 설명은 <https://velog.io/@ajufresh/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%A9%80%EC%A9%A1%ED%95%9C-%EC%82%AC%EA%B0%81%ED%98%95-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-Java> 이 블로그를 참고하였습니다.
- 따라서 2,3 를 기준으로 최대공약수를 구해 그만큼 또 곱해주면 사용할 수 없는 블럭의 갯수를 구할 수 있습니다.





-----  

## 4. 접근 방법을 적용한 코드


```python
# 유클리드 호제법
def gcd(m,n):
    while n != 0:
       t = m%n
       (m,n) = (n,t)
    return abs(m)
def solution(w,h):
    gcdnum =gcd(w,h)
    answer = w * h - ((w //gcdnum + h//gcdnum - 1) * gcdnum)
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
