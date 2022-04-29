---
title: leetcode(리트코드)-331 Verify Preorder Serialization of a Binary Tree(PYTHON)
author: 강민석
date: 2021-08-26 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 331 - Verify Preorder Serialization of a Binary Tree  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/verify-preorder-serialization-of-a-binary-tree/> 

![](/assets/img/sample/leetcode/331/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/331/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- str로 prorder를 시킨 Tree의 요소값들이 들어옵니다.
- 위의 정보를 가지고 이진트리를 구성할 수 있으면 True 아니면 False를 리턴하세요.
- discuss를 참조하였습니다. preorder를 기준으로 트리를 만들 때 빈 자식의 개수는 유효한 자식의 갯수 + 1이라는 사실을 이용한 코드입니다.





-----  

## 5. code  



**python**

```python
class Solution:
    def isValidSerialization(self, preorder: str) -> bool:
        tmp = preorder.split(',')
        slot = 1 
        for i in tmp : 
            if slot == 0 : 
                return False
            if i == '#' : 
                slot -= 1 
            else : 
                slot += 1
        return True if slot ==0 else False
                
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/331/result.JPG)  


- 어떠한 규칙이 있을거라 생각했는데, 이런 규칙이 있는줄 몰랐네요.


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


