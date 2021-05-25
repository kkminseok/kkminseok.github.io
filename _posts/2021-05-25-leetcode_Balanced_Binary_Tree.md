---
title: leetcode(리트코드)-110 Balanced Binary Tree(python)
author: 강민석
date: 2021-05-25 03:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 110 - Balanced Binary Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/balanced-binary-tree/> 

![](/assets/img/sample/leetcode/110/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/110/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 트리의 모든 자식의 높이가 1이상 차이가 날 경우 False 아닌 경우 True를 리턴합니다.
- 직관적인 문제입니다.
- 재귀를 잘 짜지못하여 discuss를 봤습니다. 추후 다시 풀어야겠습니다.



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
    def checkdepth(self,root : TreeNode) -> int:
        if root : 
            left = self.checkdepth(root.left)
            if left == -1 :
                return -1
            right = self.checkdepth(root.right)
            if right == -1 :
                return -1
            if abs(left-right) > 1 :
                return -1
            return max(left,right) +1
        else : 
            return 0
        return count
    def isBalanced(self, root: TreeNode) -> bool:
        res = self.checkdepth(root)
        return False if res == -1 else True
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/110/result.JPG)  

필요시 c++로 짜드리겠습니다.



