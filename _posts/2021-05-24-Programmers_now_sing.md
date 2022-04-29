---
title: Programmers_2018 KAKAO BLIND RECRUITMENT[3차] 방금그곡(python)
author: 강민석
date: 2021-05-24 06:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -방금그곡 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/17683#>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
KAKAO BLIND RECRUITMENT[3차] 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- KMP으로 풀었습니다.
- '#'에 대한 처리가 굉장히 힘들었습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python

#KMP table 만들기
def maketable(m):
    size = len(m)
    li = [0] * size
    j = 0
    for i in range(1,size):
        while j > 0 and m[i] != m[j] : 
            j = li[j-1]
        if m[i] == m[j] :
            j+=1
            li[i] =j
    
    return li

#kmp
def KMP(m,li,table):
    msize = len(m)
    lisize = len(li)
    j = 0
    for i in range(lisize):
        while j > 0 and li[i] != m[j] : 
            j = table[j-1]
        if li[i] == m[j] : 
            if j == msize-1 :
                #마지막인데, 만약 비교대상의 뒤가 #인경우 
                # 즉 패턴은 ABC인데, 비교대상이 ABC#인 경우 업데이트하고 걸러줌. 
                if i+1 <lisize and li[i+1] =="#":
                    j= table[j]    
                else:
                    return True
            else :
                j+=1
    return False
    

# 총 시간을 계산합니다.    
def calcutime(start,end):
    starthour,startmin = start.split(":")
    endhour,endmin = end.split(":")
    hour = int(endhour) - int(starthour)
    mini = int(endmin) - int(startmin)
    return hour*60 + mini
        
def solution(m, musicinfos):

    answer = ""
    anstime = 0
    # m(패턴)에 대한 테이블을 미리 만들어둡니다.
    table = maketable(m)
    for i in range(len(musicinfos)):
        start,end,name,li = musicinfos[i].split(',')
        totaltime = calcutime(start,end)
        #temp는 #을고려해서 문자열을 재배치한 문자열입니다.
        temp= ""
        lisize = len(li)
        idx = 0
        #재배치 과정
        for k in range(totaltime) : 
            temp += li[idx%lisize]
            idx+=1
            if(li[idx%lisize] == "#" ) :
                temp += "#"
                idx+=1
        #print(start,end,name,temp,totaltime)
        #KMP
        if KMP(m,temp,table):
            #만약 들어온 값보다 재생시간이 더 큰값이 들어오면 갱신합니다. 같은 경우는 어차피 이미 들어온값이 작거나 먼저나온 것이므로.. 고려 안합니다.
            if anstime < totaltime : 
                anstime = totaltime
                answer = name
    #None            
    if answer =="":
        return "(None)"
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
