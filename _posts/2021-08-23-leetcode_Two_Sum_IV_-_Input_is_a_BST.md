---
title: leetcode(리트코드)-653 Two Sum IV - Input is a BST(PYTHON)
author: 강민석
date: 2021-08-23 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 653 - Two Sum IV - Input is a BST  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/two-sum-iv-input-is-a-bst/> 

![](/assets/img/sample/leetcode/653/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/653/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Tree가 주어집니다.
- 트리에서 2개의 요소를 고를 때 그 합이 k와 같은 요소가 존재한다면 True를, 없으면 False를 리턴하세요.





-----  

## 5. code  

코드설명

- 탐색하면서 해당 값이 존재하는 지 찾는 알고리즘을 작성하면 더 빠르게 결과가 나오겠지만, 비슷한 문제는 많으므로 이 코드에서 고치진 않겠습니다. Eazy는 Eazy하게 빠르게 푸는 것.!!


**python**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findTarget(self, root: Optional[TreeNode], k: int) -> bool:
        dic = {}
        def dfs(root):
            if root : 
                dic[root.val] = 1
                dfs(root.left)
                dfs(root.right)
                
        dfs(root)
        if len(dic) == 1 : 
            return False
        for i in dic : 
            if k - i != i and (k - i   in dic) : 
                return True
        return False
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1448/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


