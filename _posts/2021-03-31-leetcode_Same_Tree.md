---
title: leetcode(리트코드)100-Same Tree
author: 강민석
date: 2021-03-31 00:00:20 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 100 - Same Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/same-tree/>  

![](/assets/img/sample/leetcode/100/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/100/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- p로 들어온 트리와 q로 들어온 트리가 같은지 확인합니다.
- 모양과 값이 같아야 같은 트리입니다.


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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p==nullptr && q==nullptr) return true;
        else if(p==nullptr || q==nullptr) return false;
        else{
            if(p->val != q->val) return false;
            return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
        }
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/100/result.JPG)  






