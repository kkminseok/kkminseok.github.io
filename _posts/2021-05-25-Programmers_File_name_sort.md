---
title: Programmers_2018 KAKAKO BLIND RECURITMENT[3차] 파일명 정렬(python)
author: 강민석
date: 2021-05-25 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -파일명 정렬 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17686>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
KAKAO BLIND RECURITMENT[3차] 문제입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 직관적으로 풀었습니다.
- 정렬 조건이 2개 존재합니다.
- 문자열은 사전순, 숫자는 오름차순으로 정렬해야해서 따로따로 떼줍니다.
- 뒤의 문자(tail)은 생각하지 않았습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python
#문자열을 잘라줍니다. 자른곳까지의 문자들과 인덱스를 리턴합니다.
#인데스를 리턴하는 이유는, 다음에 찾아야할 숫자에 대한 정보를 빠르게 찾기 위함입니다.
def dif_chat(string) : 
    res = ""
    i=0
    for i in range(len(string)):
        if string[i].isdigit():
            return res.lower(),i
        res+=string[i]
    return res.lower(),i
# 위의 함수에서 받은 인덱스 정부터 끝까지 돌면서 정수인경우 값을 임시 문자열에 넣고 반환합니다.    
def dif_num(string,idx):
    res = ""
    while idx < len(string) and string[idx].isdigit():
        res+= string[idx]
        idx+=1
    return res
    
def solution(files):
    answer = []
    tp =[]
    for i in range(len(files)):
        # filestr에는 문자들이, idx에는 자른 위치에대한 정보값이 들어가 있습니다.
        filestr,idx = dif_chat(files[i])
        # filenum에는 숫자들이 문자열형태로 들어있습니다.
        filenum = dif_num(files[i],idx)
        # 이런식으로 하면 0012는 12로 변환되어 무시할 수 있게됩니다.
        filenum = int(filenum)
        # Key값 정렬을 위해 tuple로 넣어줬습니다.
        temptuple = (filestr,files[i],filenum)
        tp.append(temptuple)
        #print(tp)
    # tup[0]에 대한 정렬을 먼저한 뒤, tup[2]에대해 정렬을 합니다.
    # tup[0]는 filestr로 자른 문자열, tup[2]에는 숫자에대한 정보가 들어있습니다.
    tp = sorted(tp, key=lambda tup: (tup[0],tup[2]))   
    # 정렬된 뒤 tp[i][1]에는 원본 문자열이 들어있으므로 해당 값을 결과값에 넣어줍니다.
    for i in range(len(tp)):
        answer.append(tp[i][1])
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
