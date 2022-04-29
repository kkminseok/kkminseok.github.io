---
title: leetcode(리트코드)-1448 Count Good Nodes in Binary Tree(PYTHON)
author: 강민석
date: 2021-08-18 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1448 - Count Good Nodes in Binary Tree  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-good-nodes-in-binary-tree/> 

![](/assets/img/sample/leetcode/1448/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1448/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Tree가 주어집니다.
- 상위 부모노드의 val값보다 큰 자식의 val값의 갯수를 찾아 리턴합니다.





-----  

## 5. code  

코드설명


**python**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        result = [0]
        def dfs(root : TreeNode, sd,result) -> int : 
            if root : 
                if sd <= root.val : 
                    sd = root.val
                    result[0] += 1
                    print(result,root.val,sd)
                dfs(root.left,sd,result) 
                dfs(root.right,sd,result)
        dfs(root,root.val,result)
        return result[0]
                    
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1448/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


