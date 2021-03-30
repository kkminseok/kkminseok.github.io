---
title: leetcode(리트코드)3월28일 challenge971-Flip Binary Tree To Match Preorder Traversal
author: 강민석
date: 2021-03-30 12:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 28일 - Flip Binary Tree To Match Preorder Traversal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/>  

![](/assets/img/sample/leetcode/971/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/971/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 28일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 주어진 트리속에서 자식을 swap하여 preorder했을 때 voyage에 들어온 값과 맞으면 swap한 자식들을 모은 벡터를 리턴합니다.




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
    bool check(TreeNode* root,vector<int>& voyage, int& count,vector<int>& result){
        if(root==nullptr) return true;
        if(root->val != voyage[count++]) return false;
        auto left = root->left;
        auto right = root->right;
        if(left!=nullptr && left->val != voyage[count]){
            result.push_back(root->val);
            swap(left,right);
        }
        return check(left,voyage,count,result) && check(right,voyage,count,result);
    }
    
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        int count = 0;
        vector<int> result;
        if(check(root,voyage,count,result)){
            return result;
        }
        return vector<int>() = {-1};
            
        
    }
};

```


-----

## 6. 결과 및 후기, 개선점

**c++ 74%**
![](/assets/img/sample/leetcode/971/result.JPG)  

