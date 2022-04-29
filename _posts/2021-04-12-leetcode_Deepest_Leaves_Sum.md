---
title: leetcode(리트코드)4월11일 challenge1302-Deepest Leaves Sum
author: 강민석
date: 2021-04-11 09:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 11일 - Deepest Leaves Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/deepest-leaves-sum/>  

![](/assets/img/sample/leetcode/1302/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1302/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 11일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 트리의 가장 깊은 자식의 노드값들을 더해 리턴하면됩니다.





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
    int deepestLeavesSum(TreeNode* root) {
        int res = 0 ,i;
        queue<TreeNode*> q;
        q.push(root);
        
        while(!q.empty()){
            for(res = 0, i = q.size()-1; i>=0; --i){
                TreeNode* nd = q.front();
                q.pop();    
                res += nd->val;
                if(nd->left) q.push(nd->left);
                if(nd->right) q.push(nd->right);
            }
        }
        return res;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

discuss 봄.


