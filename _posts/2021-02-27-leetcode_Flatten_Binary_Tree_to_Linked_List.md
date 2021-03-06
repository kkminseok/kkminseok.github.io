---
title: leetcode(리트코드)114-Flatten Binary Tree to Linked List
author: 강민석
date: 2021-02-27 16:30:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 55 - Flatten Binary Tree to Linked List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/flatten-binary-tree-to-linked-list/>  

![](/assets/img/sample/leetcode/114/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/114/input.JPG)  

**Constraints:**

- The number of nodes in the tree is in the range [0, 2000].
- -100 <= Node.val <= 100
 

- Follow up: Can you flatten the tree in-place (with O(1) extra space)?

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 주어진 트리를 오른쪽으로 펼쳐놓아야합니다.


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
    void Preorder(TreeNode* Node,TreeNode*& temp)
    {
        if(Node==nullptr)
            return ;
        
        Preorder(Node->right,temp);
        Preorder(Node->left,temp);
        Node->right = temp;
        Node->left=nullptr;
        temp = Node;

    }
    void flatten(TreeNode* root) {
        TreeNode* temp=nullptr;
        Preorder(root,temp);
    }
};
```


-----

## 6. 결과 및 후기, 개선점


Discuss를 참고했습니다.  
따라서 수행시간은 적지 않겠습니다.  
temp라는 변수를 통해 오른쪽부터 순회를 하면서 값들을 저장합니다. 결과적으로 오른쪽 자식부터 돌면서 중간에 값들을 끼워넣는 방식입니다.  


