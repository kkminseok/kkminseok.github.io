---
title: leetcode(리트코드)94-Binary Tree Inorder Traversal
author: 강민석
date: 2021-02-15 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 94 - Binary Tree Inorder Traversal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/binary-tree-inorder-traversal/>  

![](/assets/img/sample/leetcode/94/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/94/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 특별할 게 없는 Inorder 문제입니다.

-----  

## 5. code

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
    vector<int> result;
    vector<int> inorderTraversal(TreeNode* root) {
        if(root!=nullptr)   
        {
            inorderTraversal(root->left);
            result.push_back(root->val);
            inorderTraversal(root->right);
        }
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(100%)**  

![](/assets/img/sample/leetcode/94/result.JPG)
