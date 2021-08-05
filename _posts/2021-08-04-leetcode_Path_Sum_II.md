---
title: leetcode(리트코드)-113 Path Sum II(PYTHON)
author: 강민석
date: 2021-08-04 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 113 - Path Sum II  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/path-sum-ii/> 

![](/assets/img/sample/leetcode/113/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/113/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Tree가 주어지고, targetSum이 주어집니다.
- Tree들의 자식들을 따라 내려가면서 값들을 더할 때 targetsum이 되고, 마지막 노드인 경우 해당 경로를 리스트에 담아 리턴하세요.




-----  

## 5. code  

코드설명
- 자식들을 따라가면서 경로를 temp에 담아주고, 자식노드들에게 임시합들을 더해줍니다. 
- 만약 임시 합이 targetSum과 같고, 마지막노드인 경우(root.left == None and root.right == None) temp리스트에 저장된 경로들을 결과 리스트에 넣고 리턴합니다.



**python**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> List[List[int]]:
        res = []
        if root == None :
             return None
        if root.left == None and root.right == None : 
            if root.val == targetSum : 
                res.append([root.val])    
            return res
        def DFS(root : TreeNode, val, targetSum,dis,res) :
            if root :
                dis.append(root.val)
                root.val = root.val + val
                if root.val == targetSum and root.left == None and root.right ==None: 
                    res.append(dis)
                else : 
                    DFS(root.left,root.val,targetSum,dis[:],res)
                    DFS(root.right,root.val,targetSum,dis[:],res)
        
        
        DFS(root.left,root.val,targetSum,[root.val],res)
        DFS(root.right,root.val,targetSum,[root.val],res)
        return res        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/113/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


