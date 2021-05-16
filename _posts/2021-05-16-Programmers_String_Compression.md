---
title: Programmers_문자열 압축 Python
author: 강민석
date: 2021-05-16 03:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -문자열 압축 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/60057?language=python3#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2020 KaKao Blind Recuritment 문제입니다.
Level 2난이도의 문제입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제 내용 자체는 어렵지 않습니다.
- 연속된 경계를 검사해줘야합니다.
- 저는 list에 자른 값들을 저장해 놓고 하나씩 꺼내어 비교해가는 방법을 사용했습니다.
- 비교를 편하게 하기 위해 마지막에 더미 값을 하나 집어넣었습니다.






-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(s):
    answer = len(s)
    
    #반 이상 넘어갈 일이 없습니다.
    for i in range(1,len(s)//2+1):
        list = []
        # 1개 ~ 문자열의 길이 반 만큼 자르고 list에 넣습니다.
        for j in range(i,len(s)+1,i):
            list.append(s[j-i:j])
            # 만약 다음 값이 범위를 벗어난 경우 마지막 값을 추가해줍니다.
            if j+i > len(s) :
                list.append(s[j:])
        # 더미 노드를 넣습니다.
        if(list[:-1] != '') : 
            list.append('')
        #print(list)
        #반복되는 값을 저장하기 위한 변수입니다.
        countnum = 1
        #자릿수마다 나오는 값을 임시로 저장하기 위한 변수입니다.
        tempres = 0
        # list를 돕니다.
        for j in range(1,len(list)):
            # 만약 반복되는 값이라면 count값을 늘려줍니다.
            if list[j] == list[j-1] : 
                countnum+=1
            else:
                #print("else",j,list[j-1])
                # 1인 경우는 생략해야하므로 자릿수를 더하지 않습니다.
                if countnum == 1:
                    tempres += len(list[j-1])
                else:
                    #자릿수를 계산하기 위해 string으로 바꿔줍니다.
                    temp = str(countnum)
                    # 자릿수와 자른 문자열의 크기를 더해갑니다.
                    tempres += (len(temp) +len(list[j-1]))
                # 초기화 해줍니다.
                countnum=1
        #print(i, tempres)
        # 값을 갱신해줍니다.
        answer = min(tempres,answer)
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
