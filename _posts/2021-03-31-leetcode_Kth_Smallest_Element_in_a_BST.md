---
title: leetcode(리트코드)230-Kth Smallest Element in a BST
author: 강민석
date: 2021-03-31 02:04:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 230 - Kth Smallest Element in a BST 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/kth-smallest-element-in-a-bst/>  

![](/assets/img/sample/leetcode/230/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/230/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 이진트리에서 k번째로 작은 값을 리턴하세요.
- set으로 풀었지만 재귀로 풀어야할 것 같아서 밑에 코드를 첨부하겠습니다.



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
    void findst(set<int>& st, TreeNode* root){
        if(root!=nullptr){
            st.insert(root->val);
            findst(st,root->left);
            findst(st,root->right);
        }
    }
    
    int kthSmallest(TreeNode* root, int k) {
        set<int> st;
        findst(st,root);
        int count = 1;
        for(auto it = st.begin();it!=st.end(); ++it,++count)
            if(count==k){
                return *it;
            }
        
        return 1;
        
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 50% python ??%** 
![](/assets/img/sample/leetcode/230/result.JPG)  


**재귀 코드c++**

```c++
class Solution {
public:
    void inorder(TreeNode* root, vector<int> &res){
        if(!root)
            return;
        inorder(root->left, res);
        res.push_back(root->val);
        inorder(root->right,res);
        
    }
    int kthSmallest(TreeNode* root, int k) {
        if(!root)
            return -1;
        vector<int> arr;
        inorder(root, arr);
        return arr[k-1];

    }
};
```

