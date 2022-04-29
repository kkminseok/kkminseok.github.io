---
title: leetcode(리트코드)-145 Binary Tree Postorder Traversal(python)
author: 강민석
date: 2021-05-26 04:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 145 - Binary Tree Postorder Traversal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/binary-tree-preorder-traversal/> 

![](/assets/img/sample/leetcode/145/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/145/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- postorder해서 값들을 리스트에 담고 리턴합니다.



-----  

## 5. code  

코드설명

직관적이므로 따로 설명하지 않겠습니다. 모르겠는 분은 질문남겨주세요.

**python**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def dfs(self,root,res) :
        if root : 
            self.dfs(root.left,res)
            self.dfs(root.right,res)
            res.append(root.val)
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        res= []
        self.dfs(root,res)
        return res
        
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/145/result.JPG)  

필요시 c++로 짜드리겠습니다.



