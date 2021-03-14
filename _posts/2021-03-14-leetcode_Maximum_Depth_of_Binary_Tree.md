---
title: leetcode(리트코드)104-Maximum Depth of Binary Tree
author: 강민석
date: 2021-03-14 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 104 - Maximum Depth of Binary Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-depth-of-binary-tree/>  

![](/assets/img/sample/leetcode/104/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/104/input.JPG)  

**Constraints:**

- The number of nodes in the tree is in the range [0, 104].
- -100 <= Node.val <= 100

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 트리의 깊이를 찾아 리턴합니다.

-----  

## 5. code

**python**


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if root == None : 
            return 0
        return max(self.maxDepth(root.left),self.maxDepth(root.right))+1
```

------  

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
    int cal(TreeNode* root)
    {
        if(root!=nullptr)
        {
            return max(cal(root->left),cal(root->right))+1;
        }
        return 0;
    }
    
    int maxDepth(TreeNode* root) {
        return cal(root);
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python 98%**  
![](/assets/img/sample/leetcode/104/result.JPG)  

 
