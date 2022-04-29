---
title: leetcode(리트코드)2월2일 challenge669-Trim a Binary Search Tree
author: 강민석
date: 2021-02-11 15:06:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Tree]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge02 - Trim a Binary Search 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/trim-a-binary-search-tree/>  

![](/assets/img/sample/leetcode/669/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/669/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월2일자 챌린지 문제입니다. 
leetcode를 2월 9일부터 시작해서 늦게나마 풉니다.  

-----  

## 4. 문제 해석

- tree와 low와 high를 줍니다.
- low보다 작거나 high보다 큰 값을 트리에서 잘라내야합니다.
- 재귀에 익숙하지 않아서 트리가 쳐내지지 않아 다른 사람들의 코드를 참고하였습니다.


-----  

## 5. code

```c++
c++
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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(root!=nullptr)
        {
            if(root->val >=low && root->val<=high)
            {
                //왼쪽을 검사
                root->left = trimBST(root->left,low,high);
                //오른쪽을 검사
                root->right = trimBST(root->right,low,high);
                //잘 마무리되면 root return
                return root;
            }
            //값이 작은데 오른쪽 자식이 있다면 검사를 한 뒤 리턴
            if(root->val<low)
                return trimBST(root->right,low,high);
            //값이 작은데 왼쪽 자식이 있다면 검사를 한 뒤 리턴
            return trimBST(root->left,low,high);
        }
        return root;
    }
};

```
-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/669/result.JPG) 


**시간효율성이 좋은 코드**

위의 코드와 비슷합니다.  

재귀가 아직 약하다는 것을 알았습니다.  