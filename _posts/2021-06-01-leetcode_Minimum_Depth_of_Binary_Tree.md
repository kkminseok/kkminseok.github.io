---
title: leetcode(리트코드)-111 Minimum Depth of Binary Tree(python)
author: 강민석
date: 2021-06-01 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 111 - Minimum Depth of Binary Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-depth-of-binary-tree/> 

![](/assets/img/sample/leetcode/111/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/111/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 트리의 자식 노드중에서 가장 작은 Level에 있는 자식을 찾아 그 Level를 리턴합니다.



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
    def minDepth(self, root: TreeNode) -> int:
        if root :
            left = self.minDepth(root.left)
            right = self.minDepth(root.right)
            #자식이 두 개인 경우와 아닌 경우를 분리해서 판단합니다.
            return left + right + 1 if left == 0 or right == 0 else min(left,right) + 1
        else : 
            return 0
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/111/result.JPG)  

필요시 c++로 짜드리겠습니다.



