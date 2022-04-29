---
title: leetcode(리트코드)-1720 Decoded XORed Array(PYTHON)
author: 강민석
date: 2021-08-06 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1720 - Decoded XORed Array  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/decode-xored-array/> 

![](/assets/img/sample/leetcode/1720/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1720/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- encoded는 어떠한 리스트들의 인접한 값들끼리 XOR을하여 나온 결과입니다.
- first는 어떠한 리스트의 첫 번째 요소일 때 XOR하기 전의 리스트를 구해 리턴하세요.

-----  

## 5. code  

코드설명

- 요소와 결과로 나온 encoded의 리스트의 요소를 다시 XOR연산을 해주면 XOR 해주기 전의 값이 나옵니다.


**python**

```python
class Solution:
    def decode(self, encoded: List[int], first: int) -> List[int]:
        res = [first]
        for i in range(len(encoded)):
            res.append(encoded[i]^ res[i])
        return res
        
                     
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1725/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


