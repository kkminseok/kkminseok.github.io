---
title: Programmers_Summer/Winter Coding(~2018) 스킬트리(python)
author: 강민석
date: 2021-05-24 01:34:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -스킬트리 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/49993>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
Summer/Winter Coding(~2018) 문제입니다.
Level 2난이도의 문제입니다. 


-----  

## 3. 생각한 것들(문제 접근 방법)

- brute하게 풀면 됩니다. 예외처리를 잘 생각해야합니다.
- 선행스킬을 찾지 못한 경우에서 다음 스킬은 찾은 경우 값을 세주지 않았습니다.


-----  

## 4. 접근 방법을 적용한 코드


```python
def solution(skill, skill_trees):
    answer = 0
    for i in range(len(skill_trees)):
        #skill_tree는 들어온 문자열 중 하나입니다.
        skill_tree = skill_trees[i]
        check = True
        minN = -1
        for j in range(len(skill)):
            # 주어진 스킬트리에서 기존 선행 스킬을 찾습니다.
            findidx = skill_tree.find(skill[j])
            # 찾지 못한 경우 False로 저장해놓습니다. 만약 다음 스킬을 찾아버리면 반복문을 탈출합니다.
            if findidx == -1 :
                check = False
            else:
                # 찾았지만, 선행스킬보다 스킬을 먼저 배우는 경우 탈출합니다.
                if minN >findidx :
                    check = False
                    break
                # 찾았는데, 선행스킬을 배우지 않은 경우 탈출합니다.
                elif check == False:
                    break
                check = True
                minN = findidx
            #완벽하게 수행했으면 answer+=1을 해줍니다.
            if j == len(skill)-1 :
                answer+=1
                
    return answer
```


-----



## 5. 결과

필요시. c++ 짜드리겠습니다.















 
