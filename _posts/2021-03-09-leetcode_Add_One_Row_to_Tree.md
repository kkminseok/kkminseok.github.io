---
title: leetcode(리트코드)3월09일 challenge623-Add One Row to Tree
author: 강민석
date: 2021-03-09 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 09일 - Add one Row to Tree문제입니다.**

## 1. 문제
<https://leetcode.com/problems/add-one-row-to-tree/>  

![](/assets/img/sample/leetcode/623/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/623/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 09일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- Tree와 v와 d가 인풋으로 들어옵니다. d의 깊이에서 v의 값을 가진 노드들을 집어 넣어야합니다. 
- d깊이에 넣어줘야하므로 d-1를 감지해야합니다. d가 1로 들어올 경우도 생각해야합니다.



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
    
    def addOneRow(self, root: TreeNode, v: int, d: int) -> TreeNode:
        def order(root : TreeNode, v : int , curr : int, d :int) :
            if root != None : 
                if d-1 == curr:
                    if root.left != None  :
                        newNode = TreeNode(v)
                        newNode.left = root.left
                        root.left = newNode
                    else :
                        newNode = TreeNode(v)
                        root.left = newNode
                    if root.right != None : 
                        newNode = TreeNode(v)
                        newNode.right = root.right
                        root.right = newNode
                    else : 
                        newNode = TreeNode(v)
                        root.right = newNode
                order(root.left,v,curr+1,d)
                order(root.right,v,curr+1,d)
        if d == 1:
            newNode = TreeNode(v)
            newNode.left = root
            return newNode
        else :
             order(root,v,1,d)
        return root
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
    void order(TreeNode* root, int v,int cur,int d)
    {
        if(root!=nullptr)
        {
            if(d-1 ==cur)
            {
                if(root->left!=nullptr)
                {
                    TreeNode* newNode =new TreeNode(v);
                    newNode->left = root->left;
                    root->left = newNode;
                }
                else
                {
                    TreeNode* newNode= new TreeNode(v);
                    root->left = newNode;
                }
                if(root->right!=nullptr)
                {
                    TreeNode* newNode =new TreeNode(v);
                    newNode->right = root->right;
                    root->right = newNode;
                }
                else
                {
                    TreeNode* newNode = new TreeNode(v);
                    root->right = newNode;
                }
            }
            order(root->left,v,cur+1,d);
            order(root->right,v,cur+1,d);
        }
    }
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        if(d==1)
        {
            TreeNode* newNode = new TreeNode(v);
            newNode ->left = root;
            return newNode;
        }
        else
            order(root,v,1,d);   
        return root;
    }
};

```

-----

## 6. 결과 및 후기, 개선점

**c++ 90%**  
![](/assets/img/sample/leetcode/623/result.JPG)  


**8ms 100% code**

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
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        if (! root) return NULL;
        bool level_done = false;
        
        if (d == 1) {
            TreeNode *node = new TreeNode(v, root, NULL);
            return node;
        }
        int level = 1;
        queue <TreeNode *>q;
        
        q.push(root);
        while (!q.empty()) {
            int sz = q.size();
            for (int i = 0; i < sz; i ++) {
                TreeNode *node = q.front();
                q.pop();
                
                if(level == d - 1) {
                    TreeNode *node1 = new TreeNode(v, node->left, NULL);
                    TreeNode *node2 = new TreeNode(v, NULL, node->right);
                    node->left = node1;
                    node->right = node2;
                    level_done = true;
                    //return root;
                } else {
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
                }
            }
            if (level_done) return root;
            level += 1;
        }
        return root;
    }
};
```

BFS를 이용해서 level를 찾고 level-1에서 새로운 노드들을 넣는 작업을 수행합니다.



