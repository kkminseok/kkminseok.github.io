---
title: leetcode(리트코드)2월9일 challenge538-Convert BST to Greater Tree
author: 강민석
date: 2021-02-09 16:02:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Tree]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge09 - Convert BST to Greater Tree문제입니다.**

## 1. 문제
<https://leetcode.com/explore/challenge/card/february-leetcoding-challenge-2021/585/week-2-february-8th-february-14th/3634/>  

![](/assets/img/sample/leetcode/February/9/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/February/9/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium의 난이도입니다.  
2월9일자 챌린지 문제입니다. 코드는 어렵지 않으나..

-----  

## 4. 문제 해석

- right-> root-> left를 돌면서 값을 더해줘야합니다.  
- 일반적으로 알고 있는 left->root->right가 아니라 처음에는 풀기 까다로웠습니다. 
- sum이라는 변수를 선언해서 재귀를 돌면서 관리해줘야합니다.  




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
    int sum=0;
    void order(TreeNode* pointer,int& sum){
        if(pointer!=nullptr)
        {
            order(pointer->right,sum);
            sum += pointer->val;
            pointer->val=sum;
            order(pointer->left,sum);
        }
        
    }

    TreeNode* convertBST(TreeNode* root) {
        order(root,sum);
        return root;
    }
};
```
-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/February/9/result.JPG) 


**시간효율성이 좋은 코드**

제 코드와 같습니다.  
  
**공간 효율성이 좋은 코드** 

위의 코드와 같아서 따로 적진 않겠습니다.  
