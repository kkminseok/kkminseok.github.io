---
title: leetcode(리트코드)102-Binary Tree Level Order Traversal
author: 강민석
date: 2021-02-15 13:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 102 - Binary Tree Level Order Traversal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/binary-tree-level-order-traversal/>  

![](/assets/img/sample/leetcode/102/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/102/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 트리를 level별로 2차원vector에 넣어 반환해야합니다.

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
    //결과 벡터
    vector<vector<int>> result;
    vector<vector<int>> order(TreeNode* root,int count)
    {
        if(root!=nullptr)
        {
            //만약 처음 들어온 깊이라면 vector를 생성하여 result 벡터에 넣어줍니다.
            if(count>result.size())
            {
                vector<int> temp;
                temp.push_back(root->val);
                result.push_back(temp);
            }
            //처음 들어온 깊이가 아니라면 기존 vector가 있다는 것이므로 결과값에 넣습니다.
            else
                result[count-1].push_back(root->val);
            order(root->left,count+1);
            order(root->right,count+1);
        }
        
        return result;   
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        result = order(root,1);
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(8%)**  

![](/assets/img/sample/leetcode/102/result.JPG)

이게 왜 8%가 나오는 지 몰라서 Discuss에 올렸습니다.

**0ms(100%) 코드**

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        //결과 벡터
        vector<vector<int>> res;
        //Node를 담을 큐
        queue<TreeNode*> q_node;
        if (root == NULL) {
            return {};
        }
        //root가 null이 아니라면 queue에 넣습니다.
        q_node.push(root);
        while (!q_node.empty()) {
            vector<int> level_nodes;
            int size = q_node.size();
            for (int i = 0; i < size; i++) {
                //tmp는 임시 노드입니다.
                TreeNode* tmp = q_node.front();
                //임시 노드를 벡터에 넣습니다.
                level_nodes.push_back(tmp->val);
                //left가 비어있지 않으면 큐에 넣고
                if (tmp->left != NULL) {
                    q_node.push(tmp->left);
                }
                //right가 비어있지 않으면 마찬가지로 큐에 넣습니다.
                if (tmp->right != NULL) {
                    q_node.push(tmp->right);
                }
                // Don't forget to pop q_node INSIDE the for loop after visit it.
                q_node.pop();
            }
           
            res.push_back(level_nodes);
        }
        return res;
    }
};
```
 
자식들을 한번에 넣고 처리하는 방식입니다. queue를 쓰는 방법을 배웠습니다. queue에는 예제 기준으로 3, 9, 20, 15, 7 이렇게 배열처럼 들어갈 것입니다.