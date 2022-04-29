---
title: leetcode(리트코드)6월02일 challenge97-Interleaving String(python)
date: 2021-06-02 04:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 02일 - Interleaving String 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/interleaving-string/>  

![](/assets/img/sample/leetcode/97/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/97/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 02일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 두 개의 문자열이 들어옵니다. 각 문자열을 적절히 조합해서 s3라는 문자열을 만들 수 있으면 True를 리턴하고 만들 수 없으면 False를 리턴합니다.

- s1에서 뽑은 경우와 s2에서 뽑은 경우를 계산해야해서 DP를 사용합니다. Discuss를 보고 힌트를 얻었습니다. 이러면 안되는데..

- DP방식은 염기서열 문제와 비슷합니다.



-----  

## 5. code


**python**

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        row = len(s1)+1
        col = len(s2)+1
        if row + col-2 != len(s3)  : 
            return False
        DP = [0] * (row)
        for i in range(row) : 
            DP[i] = [0] * (col)
        for i in range(row) : 
            for j in range(col):
                if i == 0 and j == 0:
                    DP[i][j] = 1
                elif i == 0 :
                    DP[i][j] = (1 if DP[i][j-1] and s2[j-1] == s3[i+j-1] else 0)
                elif j == 0:
                    DP[i][j] = (1 if DP[i-1][j] and s1[i-1] == s3[i+j-1] else 0)
                else : 
                    DP[i][j] = 1 if (DP[i-1][j] and s1[i-1] == s3[i+j-1]) or (DP[i][j-1] and s2[j-1] == s3[i+j-1]) else 0
        #print(DP)
        return DP[row-1][col-1]
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/97/result.JPG)  

필요시 c++로 풀어드립니다.



