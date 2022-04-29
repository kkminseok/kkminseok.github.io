---
title: leetcode(리트코드)236-Lowest Common Ancestor of a Binary Tree
author: 강민석
date: 2021-04-01 02:04:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 236 - Lowest Common Ancestor of a Binary Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/>  

![](/assets/img/sample/leetcode/236/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/236/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- P와 Q를 자식으로 갖는 가장 레벨이 큰 부모노드를 찾습니다.
- discuss를 봤습니다.


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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        //만약 찾거나 끝의 노드에 접근하면 반환합니다.
        if(!root || root ==q || root==p) return root;
        TreeNode* l = lowestCommonAncestor(root->left,p,q);
        TreeNode* r = lowestCommonAncestor(root->right,p,q);
        //부모 노드 안에서 l과 r이 있으면 root노드를 반환, 하나만 찾았으면 하나의 노드를 쭉 가지고 올라갑니다.
        return !l ? r : !r? l : root;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

discuss 코드를 보고 재귀의 힘을 느끼면서 공부를 더 많이 해야겠다는 생각을 했습니다..



