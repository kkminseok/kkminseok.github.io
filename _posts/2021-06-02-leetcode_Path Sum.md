---
title: leetcode(리트코드)-112 Path Sum(python)
author: 강민석
date: 2021-06-02 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 112 - Path Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/path-sum/> 

![](/assets/img/sample/leetcode/112/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/112/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 트리의 경로를 탔을 때 targetSum을 만들 수 있는 지  만들 수 있다면 해당 노드가 뿌리노드까지 닿은건지 확인하여 return합니다.



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
    def hasPathSum(self, root: TreeNode, targetSum: int) -> bool:
        def DFS(root,targetSum,curr):
            if root: 
                curr += root.val
                #만약 트리노드의 끝이고, targetSum과 같다면
                if curr == targetSum and root.left == None and root.right == None: 
                    return True
                return DFS(root.left,targetSum,curr) or DFS(root.right,targetSum,curr)
        return DFS(root,targetSum,0)
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/112/result.JPG)  

필요시 c++로 짜드리겠습니다.



