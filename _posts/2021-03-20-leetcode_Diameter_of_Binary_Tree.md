---
title: leetcode(리트코드)543-Diameter of Binary Tree
author: 강민석
date: 2021-03-20 01:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 543 - Diameter of Binary Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/diameter-of-binary-tree/>  

![](/assets/img/sample/leetcode/543/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/543/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 주어진 트리에서 가장 긴 경로를 가진 길이를 구하세요.
- 트리의 root를 지날 수도 안지날 수도 있습니다.
- 왼쪽자식 + 오른쪽 자식의 합을 매 트리의 노드마다 구해줘서 갱신했습니다.


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
    int result = 0;
    int order(TreeNode* node)
    {
        if(node!=nullptr)
            return max(order(node->left),order(node->right) )+1;
        return 0;
    }
    
    int diameterOfBinaryTree(TreeNode* root) {
        if(root!=nullptr)
        {
            int leftdepth = order(root->left);
            int rightdepth = order(root->right);
            result = max(result,leftdepth+rightdepth);
            diameterOfBinaryTree(root->left);
            diameterOfBinaryTree(root->right);
        }
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 50% python ??%** 
![](/assets/img/sample/leetcode/234/result.JPG)  




**c++ 0ms 100% code**

```c++
class Solution {
public:
    int ans = 0;
    int util(TreeNode *root){
        if(!root) return 0;
        int left = util(root->left);
        int right = util(root->right);
        ans = max(left + right, ans);
        return max(left, right) + 1;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        util(root);
        return ans;
    }
};
```
제 로직에서 쓸데없는 재귀를 없애버린 코드입니다.



