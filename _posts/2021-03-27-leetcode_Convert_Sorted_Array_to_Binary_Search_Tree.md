---
title: leetcode(리트코드)108-Convert Sorted Array to Binary Search Tree
author: 강민석
date: 2021-03-27 13:12:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 108 - Convert Sorted Array to Binary Search Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/>  

![](/assets/img/sample/leetcode/108/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/108/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 모든 자식 노드에 대해서 높이가 1이상 차이나지 않는 트리를 만듭니다.

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
    TreeNode* makeTree(vector<int>& nums,int left,int right){
        if(left>right)
            return nullptr;
        int mid = (left + right) / 2;
        TreeNode* newNode = new TreeNode(nums[mid]);
        newNode->left = makeTree(nums,left,mid-1);
        newNode->right = makeTree(nums,mid+1,right);
        return newNode;
    }
    
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return makeTree(nums,0,nums.size()-1);
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 61% python ??%** 
![](/assets/img/sample/leetcode/108/result.JPG)  






