---
title: leetcode(리트코드)-235 Lowest Common Ancestor of a Binary Search(PYTHON)
author: 강민석
date: 2021-07-19 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 235 - Lowest Common Ancestor of a Binary Search  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/> 

![](/assets/img/sample/leetcode/235/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/235/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- root와 p,q가 주어집니다.
- p와 q값을 찾았을 때 공통된 가장 낮은(low level) 레벨의 조상을 리턴하세요.
- Eazy문제이지만, 생각보다 어려워서 discuss를 참고하였습니다.

-----  

## 5. code  

코드설명

**python**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root : 
            return None
        if max(p.val,q.val) < root.val : 
            return self.lowestCommonAncestor(root.left,p,q)
        elif min(p.val,q.val) > root.val :
            return self.lowestCommonAncestor(root.right,p,q)
        else : 
            return root
        
        
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/235/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


