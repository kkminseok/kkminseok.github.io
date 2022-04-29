---
title: leetcode(리트코드)-1812 Determine Color of a Chessboard Square(PYTHON)
author: 강민석
date: 2021-07-18 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1812 - Determine Color of a Chessboard Square  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/determine-color-of-a-chessboard-square/> 

![](/assets/img/sample/leetcode/1812/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1812/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다. 


-----  

## 4. 문제 해석

- coordinates는 어떠한 열과 행에 대한 이름입니다.
- 그 열과 행에 해당되는 부분이 Black인지 White인지 확인하여 black이면 False를 리턴 White는 True를 리턴하세요.

-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def squareIsWhite(self, coordinates: str) -> bool:
        res = (ord(coordinates[0]) - 97) + int(coordinates[1])
        return False if res%2 == 1 else True
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1812/result.JPG)  

필요시 c++로 작성해드립니다.

모르겟으면 댓글 부탁드립니다.

