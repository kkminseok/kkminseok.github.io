---
title: Programmers_월간 코드 챌린지 시즌2 110 옮기기(python)
author: 강민석
date: 2021-06-15 02:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -110 옮기기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/77886>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
월간 코드 챌린지 시즌2 문제입니다.

Level 3난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제는 직관적이나 효율성을 많이 따지는 문제입니다.

1. 맨 처음 생각한 것은 110을 찾아서 그 앞에서 1이 끝나는 시점에 넣어주는 것입니다. 넣어준 곳을 기준으로 다시 110을 찾고 맨 끝까지 도달하거나 110을 찾을 수 없으면 반복문을 벗어납니다.
    + 예제는 위와같은 방법으로 해결할 수 있으나 다른 예제들이 틀려서 접근 방법이 틀렸습니다.

**위 방법으로 해결해본 코드**

```python
def findone(s,idx):
    while idx >= 0 and s[idx] == "1":
        idx-=1
    return idx
def solution(s):
    answer = []
    for word in s : 
        idx = word.find("110")
        while idx < len(word) and word.find("110",idx,len(word)) != -1 :
            idx = word.find("110",idx,len(word))
            findnum = findone(word,idx)
            print("~findnum",word[:findnum+1], "findnum ~idx",word[findnum+1:idx], "idx~",word[idx+3:])
            word = word[:findnum+1] + "110" + word[findnum+1 : idx] + word[idx+3 :]
            print("findnum : ",findnum, "idx : ",idx,"res : ",word)
            idx = findnum + 3
        answer.append(word)
                
    return answer
```

2. 질문하기 게시판에서 힌트를 얻었습니다.
    + 먼저 "110"을 다 빼줍니다.
    + 그 후 남은 문자열에 "11"을 찾고 그 앞에 전부 넣거나 없으면 "0"을 찾아 그 뒤에 다 넣습니다.
    + 이 부분이 보기에는 쉬우나 효율성 부분에서 많이 틀렸습니다.
    + python의 replace()함수로 "110"을 ""으로 바꾸는 것도 효율성에서 탈락했습니다. 또한, 문자열이 땡겨지면서 새로 생긴 "110"을 고려해줘야합니다.
    + 문자열을 합치는 것도 효율성에서 틀립니다. s[:idx] + s[idx+3:]이런 식의 코드는 틀립니다.
    ```python
    #초기 check는 1
    while check :
            check = word.count("110")
            count+=check
            word = word.replace("110","",check)
            #뻑납니다.
    ```
    + 많은 고민을 하고, 문자열을 stack처럼 생각하여 사용하였습니다.
    + 문자열을 돌면서 임시 문자열에 저장하다가 "0"이 나오면 앞의 두 값이 "11"일 경우 임시 문자열의 "11"을 없애주고 count를 갱신해줍니다.


-----  

## 4. 접근 방법을 적용한 코드

```python
def solution(s):
    answer = []
    
    for word in s : 
        count=0
        st = ""
        for i in range(len(word)):
            #만약 st에 쌓은 것 앞의 2개가 "11"일 경우 count를 갱신해주고, st도 빼줍니다.
            if word[i] =="0" and st[-2:] == "11":
                st = st[:-2]
                count+=1
            else : 
                st += word[i]
        
        
        idx = st.find("11") 
        #"11"을 찾고 있으면 그 다음 인덱스에 다 넣습니다.
        #없으면 rfind(뒤에서 0을 찾아)그 앞에 다 넣습니다.
        if idx == -1:
            idx2 = st.rfind("0")
            st = st[:idx2+1] + "110" * count + st[idx2+1:]
        else : 
            st = st[:idx] + "110" * count +st[idx:]
        answer.append(st)                
                
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다. 설명이 필요시 댓글달아주세요.















 
