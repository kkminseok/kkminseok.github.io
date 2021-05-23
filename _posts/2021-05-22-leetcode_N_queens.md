---
title: leetcode(리트코드)5월22일 challenge51-N Queens
date: 2021-05-21 00:01:56 +0800
categories: [leetcode,Hard]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 22일 - N Queens 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/n-queens/>  

![](/assets/img/sample/leetcode/51/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/51/input.JPG)  


-----  

## 3. 분류 및 난이도

Hard 난이도입니다.  
5월 21일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 유명한 N Queens 문제입니다.
- 대각선으로 겹치지 않게 병사들을 놓을 때 놓을 수 있는 경우의 수를 리턴합니다.
- 대학교 수업에서 배운 것이 있어서 코드를 가져다 썼습니다.(이해는 잘 못함.)


-----  

## 5. code


**python**

```python
class Solution:
    
    
    def solveNQueens(self, n: int) -> List[List[str]]:
        # 너무 깊게 재귀를 하지 않도록 제한을 걸어둡니다.
        def promising(i,col):
            k = 0
            switch = True
            while k < i and switch == True :
                if col[i] == col[k] or abs(col[i] - col[k]) == i -k :
                    switch = False
                k+=1
            return switch
        def queens(n,i,col,res): 
            if promising(i,col) : 
                if i == n-1 :
                    size = len(res)
                    res.append([])
                    for j in range(len(col)):
                        resstr = ""
                        find = col[j]
                        for k in range(len(col)):
                            if k ==find :
                                resstr+="Q"
                            else :
                                resstr+="."
                        res[size].append(resstr)
                else:
                    for j in range(0,n):
                        col[i+1] = j
                        queens(n,i+1,col,res)
        col = n *[0]
        res = []
        queens(n,-1,col,res)
        return res        

        
```



-----

## 6. 결과 및 후기, 개선점

필요시 c++로 풀어드립니다.



