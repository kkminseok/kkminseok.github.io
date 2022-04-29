---
title: leetcode(리트코드)3월05일 challenge637-Average of Levels in Binary Tree
author: 강민석
date: 2021-03-06 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 05일 - Average of Levels in Binary Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/average-of-levels-in-binary-tree/>  

![](/assets/img/sample/leetcode/637/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/637/input.JPG)  

-----  

## 3. 분류 및 난이도

eazy 난이도입니다.  
3월 05일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 트리에서 같은 레벨에 있는 값들에 대해 평균값을 벡터에 넣어 리턴합니다.



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
    def averageOfLevels(self, root: TreeNode) -> List[float]:
        result = []
        queue = deque([root])
        
        while queue:
            sumnum = 0.0
            sizenum = len(queue)
            for i in range (0,sizenum) :
                numb = queue.popleft()
                sumnum +=numb.val
                if (numb.left !=None):
                    queue.append(numb.left)
                if (numb.right !=None) :
                    queue.append(numb.right)
            result.append(sumnum/sizenum)
        return result
```

-----  

**c++**


        
```c++
/*
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
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> result;
        queue<TreeNode*> q;
        q.push(root);
        
        while(!q.empty())
        {
            int x = q.size();
            double sum = 0.0;
            for(int i = 0; i<x;++i)
            {
                TreeNode* numb = q.front();
                q.pop();
                sum += (numb->val);
                if(numb->left!=nullptr) q.push(numb->left);
                if(numb->right!=nullptr) q.push(numb->right);
            }
            result.push_back(sum/x);
        }
        return result;
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/637/result.JPG)  


