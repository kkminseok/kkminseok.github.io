---
title: leetcode(리트코드)-103 Binary Tree Zigzag Level Order Traversal
author: 강민석
date: 2021-05-03 09:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 103 - Binary Tree Zigzag Level order Traversal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/> 

![](/assets/img/sample/leetcode/103/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/103/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top Interview 문제입니다.


-----  

## 4. 문제 해석

- 트리를 level에따라 벡터에 집어넣는데, left->right, right->left 순으로 집어넣습니다.



-----  

## 5. code  

코드설명
- 일단 level에 따라 벡터에 넣고 level이 홀수인 경우에 reverse를 해줍니다.

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
    void DFS(TreeNode* root, int depth, vector<vector<int>>& res){
        if(root){
            if(res.size() < depth){
                vector<int> temp;
                temp.push_back(root->val);
                res.push_back(temp);
            }
            else
                res[depth-1].push_back(root->val);
            DFS(root->left,depth+1,res);
            DFS(root->right,depth+1,res);
        }
    }
    
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        DFS(root,1,res);
        for(int i = 0 ; i <res.size();++i){
            if(i%2==1) reverse(res[i].begin(),res[i].end());
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 61% 4ms**

![](/assets/img/sample/leetcode/103/result.JPG)  



