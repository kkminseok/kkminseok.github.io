---
title: leetcode(리트코드)226-Invert Binary Tree
author: 강민석
date: 2021-03-18 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 226 - Invert Binary Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/invert-binary-tree/>  

![](/assets/img/sample/leetcode/226/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/226/input.JPG)  

**Constraints:**

- The number of nodes in the tree is in the range [0, 100].
- -100 <= Node.val <= 100

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 주어진 트리를 대칭으로 바꿔버립니다.


-----  

## 5. code

**c++**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    
    TreeNode* invertTree(TreeNode* root) {
        if(root!=nullptr)
        {
            invertTree(root->left);
            invertTree(root->right);
            std::swap(root->left,root->right);
        }
        
        return root;
    }
};
```

-----  

**python**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if root :
            self.invertTree(root.left)
            self.invertTree(root.right)
            root.left,root.right = root.right,root.left
        return root
            
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 45% python 83%** 
![](/assets/img/sample/leetcode/226/result.JPG)  



결과는 4ms라 0ms랑 차이가 없으므로 개선사항은 없습니다.

