---
title: leetcode(리트코드)-1758 Minimum Changes To Make Altenating Binary String(PYTHON)
author: 강민석
date: 2021-08-04 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1758 - Minumum Changes To Make Altenating Binary String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-changes-to-make-alternating-binary-string/> 

![](/assets/img/sample/leetcode/1758/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1758/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 0과 1로 이루어진 문자열s가 주어집니다. 
- 0이나 1이나 연속되지 않아야 합니다. 0을 1로, 1을 0으로 바꿀 수 있을 때 최소한으로 바꾸어 원하는 문자열을 만들어야합니다.
- 최소값을 구하세요.




-----  

## 5. code  

코드설명
- '0'으로 시작했을 때 "01010101..."이 문자열의 길이만큼 될 것입니다.
- 이미 "0101010..."을 만들었다고 생각하고 틀린 문자를 찾아 그 갯수를 저장합니다.
- '1'로 시작했을 때 "1010101...."인데 위에서 구한 갯수를 s의 길이에서 빼주면 '1'로 시작했을 때의 값이 됩니다.(반전이므로)


**python**

```python
class Solution:
    def minOperations(self, s: str) -> int:
        # start 0
        # start 1
        checknum = 0 
        for i in range(len(s)):
            if i%2 == 0 :
                if s[i] == "0": 
                    continue
                checknum += 1
            else : 
                if s[i] =="1":
                    continue
                checknum += 1
        return min(checknum, len(s) - checknum)
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1758/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


