---
title: leetcode(리트코드)5월29일 challenge52-N-Queens II(python)
date: 2021-05-29 00:01:56 +0800
categories: [leetcode,Hard]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 29일 - N-Queens II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/n-queens-ii/>  

![](/assets/img/sample/leetcode/52/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/52/input.JPG)  


-----  

## 3. 분류 및 난이도

Hard 난이도입니다.  
5월 29일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 얼마 전에 푼 N-Queen 문제의 2번째 버전입니다.
- 첫 번째 문제는 리스트를 반환했지만, 이번 문제는 그 리스트의 길이를 반환하면 되는 문제라 I을 풀었으면 이번 문제는 어렵지 않게 해결할 수 있습니다.





-----  

## 5. code


**python**

```python
class Solution:
    def totalNQueens(self, n: int) -> int:
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
                        res+=1
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

![](/assets/img/sample/leetcode/52/result.JPG)  

필요시 c++로 풀어드립니다.



