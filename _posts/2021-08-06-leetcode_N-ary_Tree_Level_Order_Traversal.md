---
title: leetcode(리트코드)-429 N ary Tree Order Traversal(PYTHON)
author: 강민석
date: 2021-08-06 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 429 - N ary Tree Order Traversal  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/n-ary-tree-level-order-traversal/> 

![](/assets/img/sample/leetcode/429/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/429/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- root라는 값이 들어옵니다.
- root에는 root 값과 children이라는 리스트가 주어집니다.
- 해당 정보들로 트리를 만들 때 Level에 맞게 값들을 리스트에 넣어 반환합니다.

-----  

## 5. code  

코드설명

- alex가 이길 수 밖에 없다고 합니다. 또한, 너무 쉽게 풀려서 Medium의 레벨이 아닌 것 같습니다.
- 가장 큰 것부터 뽑아 계산하여 풀었습니다.


**python**

```python
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        res =[]
        def solution(root,depth,res):
            if root : 
                if len(res) <= depth : 
                    res.append([])
                res[depth].append(root.val)
                for i in range(len(root.children)) : 
                    solution(root.children[i],depth+1,res)
        solution(root,0,res)
        return res
        
                     
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/429/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


