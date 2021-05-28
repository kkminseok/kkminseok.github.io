---
title: Programmers_2021 KAKAKO BLIND RECURITMENT 순위 검색(python)
author: 강민석
date: 2021-05-26 00:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -순위 검색 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/72412>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2021 KAKAO BLIND RECURITMENT 문제입니다.

Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- 문제의 핵심은 효율성 부분입니다.
- 많은 사람들이 효율성 때문에 틀리고, 저 또한 효율성으로 2시간이나 잡고 있었습니다... 실전이었으면 이미 탈락.
- 다 풀고나서는 코드가 그리 길지않고 생각하는 게 어렵지 않다고 느껴서 Level 2답네 라고 했지만.. 진짜 이게 Level 2 문제인가? 싶기도 합니다.



-----  

## 4. 접근 방법을 적용한 코드

첫 코드는 정보를 튜플로 만들어서 매 인덱스마다 정렬을 해줬습니다. 틀린 코드입니다.

**처음에 삽질한 코드 효율성 제로, 틀림.**

```python
import copy
def solution(info, query):
    answer = []
    infolist = []
    for i in range(len(info)) : 
        #info tuple에 담기
        temptp = info[i].split(' ')
        temptp = (temptp[0][0],temptp[1][0],temptp[2][0],temptp[3][0],int(temptp[4]))
        infolist.append(temptp)
    #정렬
    infolist = sorted(infolist, key = lambda tp : tp[0])
        
    for i in range(len(query)):
        #query 처리문
        querytemp = query[i]
        querytemp  = querytemp.split(" and ")
        querylast = querytemp[3].split(' ')
        del querytemp[3]
        querytemp.append(querylast[0])
        querytemp.append(querylast[1])
        #print(querytemp)
        start = 0 
        end = len(info)-1
        templist = copy.deepcopy(infolist)
        for qidx in range(4):
            k = 0
            while k < len(templist):
                #print(templist)
                findlist = templist[k][qidx]
                findquery = querytemp[qidx][0]
                if findquery =="-" :
                    templist = sorted(templist, key = lambda tp : tp[qidx+1])
                    #print("templist : ",templist)
                    break
                if findlist == findquery :
                    start = k
                    while k< len(templist) and templist[k][qidx] ==  findquery :
                        k+=1
                    end = k
                    #print(start,end)
                    del templist[:start]
                    del templist[end-start:]
                    templist = sorted(templist, key = lambda tp : tp[qidx+1])
                    #print("templist : ",templist)
                else : 
                    k+=1
        #print("최종 : ",start,end,templist) 
        count = 0 
        for res in range(len(templist)):
            if int(querytemp[4]) <= templist[res][4]:
                #print(querytemp[4], templist[res][4])
                count+=1
        answer.append(count)
    return answer
```


**정답인 코드**

```python
# 쿼리문 조건에 맞는 것들을 계속 리턴합니다.
def findstr(idx,char,infolist) : 
    res = []
    for k in infolist:
        if k[idx] == char : 
            res.append(k)
    return res
# lower boundary를 이용합니다. (타겟과 같거나 처음으로 큰 값 나타나는 인덱스 리턴)
def binary_search(array,target,start,end):
    while start < end :
        mid = (start + end) //2
        if array[mid] == target : 
            end = mid
        elif array[mid] < target:
            start = mid + 1
        elif target < array[mid]:
            end = mid
    return end
            
    

def solution(info, query):
    answer = []
    dic = {}
    for i in range(len(info)) : 
        #dic에 담기
        temptp = info[i].split(' ')
        score = int(temptp[4])
        temptp= (temptp[0][0] + temptp[1][0] + temptp[2][0] + temptp[3][0])
        if temptp not in dic :
            lt = []
            dic[temptp] = lt
            dic[temptp].append(score)
        else : 
            dic[temptp].append(score)
    #나중에 이진탐색을 할 것이므로 정렬을 미리 해둡니다.
    for k in dic:
        dic[k].sort()
        
    for i in range(len(query)):
        #query 처리문
        querytemp = query[i]
        querytemp  = querytemp.split(" and ")
        querylast = querytemp[3].split(' ')
        first = querytemp[0][0] + querytemp[1][0] + querytemp[2][0] + querytemp[3][0]
        qscore = int(querylast[1])
        # 있는 경우
        if first in dic : 
            count = 0 
            for k in (dic[first]) : 
                if k >= qscore : 
                    count+=1
            answer.append(count) 
        else : 
            count = 0
            infolist = dic.keys()
            totalsize = 0
            for k in range(4) : 
                if first[k]=="-" : 
                    continue
                else :
                    infolist = findstr(k,first[k],infolist)
            for lt in infolist : 
                size = len(dic[lt])
                idx = binary_search(dic[lt],qscore,0,size)
                if size != idx:
                    count +=  (size - idx)
            answer.append(count)
            
        
        
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
