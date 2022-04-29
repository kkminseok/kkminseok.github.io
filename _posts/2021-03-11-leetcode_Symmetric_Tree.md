---
title: leetcode(리트코드)101-Symmetric Tree
author: 강민석
date: 2021-03-11 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 101 - Symmetric Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/symmetric-tree/>  

![](/assets/img/sample/leetcode/101/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/101/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 트리가 대칭인지를 판단합니다. 대칭(모양이 데칼코마니이고 값도 같아야합니다.)

-----  

## 5. code

**python**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def test(leftP : TreeNode, rightP : TreeNode) -> bool:
            if leftP == None and rightP == None : 
                return True
            elif leftP == None or rightP == None : 
                return False
            return (leftP.val == rightP.val) and test(leftP.left,rightP.right) and test(leftP.right,rightP.left)
        
        return test(root.left,root.right)
```
-----  



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
    bool test(TreeNode* leftP, TreeNode* rightP)
    {
        if(leftP==nullptr && rightP==nullptr)
            return true;
        else if(leftP==nullptr || rightP==nullptr)
            return false;
        return (leftP->val == rightP->val) &&test(leftP->left,rightP->right) && test(leftP->right,rightP->left);
    }
    bool isSymmetric(TreeNode* root) {
        return test(root->left,root->right);
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 84% python 58%**  
![](/assets/img/sample/leetcode/101/result.JPG)  

**python 12ms 100% code**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def DFS(r1, r2):
            if not r1 and not r2:
                return True
            if bool(r1) != bool(r2):
                return False
            if r1.val != r2.val:
                return False
            left = DFS(r1.left, r2.right)
            right = DFS(r1.right, r2.left)
            return left and right

        return DFS(root, root)
```
로직은 같은데 어디서 속도차이가 나는지 잘 모르겠습니다.  

