---
title: leetcode(리트코드)4월20일 challenge589-N-ary Tree Preorder Traversal
author: 강민석
date: 2021-04-20 10:17:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 20일 - N-ary Tree Preorder Traversal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/n-ary-tree-preorder-traversal/>  

![](/assets/img/sample/leetcode/589/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/589/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
4월 20일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 이진트리가 아닌 일반 트리가 들어옵니다.
- 트리를 preorder한 값을 vector에 넣어 반환합니다.




-----  

## 5. code

코드의 핵심은 자식들을 반대로 덱에 넣어줘서 자식이 있는 것을 우선순위로 꺼내는 것입니다.

**c++**

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> preorder(Node* root) {
        deque<Node*> q;
        vector<int> res;
        if(!root) return res;
        q.push_front(root);
        while(!q.empty()){
            Node* frontnode = q.front();
            q.pop_front();
            res.push_back(frontnode->val);
            for(int i = frontnode->children.size()-1 ; i >=0; --i){
                q.push_front(frontnode->children[i]);
            }
        }
        return res;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/589/result.JPG)  




