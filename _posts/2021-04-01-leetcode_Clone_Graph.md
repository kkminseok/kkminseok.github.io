---
title: leetcode(리트코드)133-Clone Graph
author: 강민석
date: 2021-04-01 03:45:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 133 - Clone Graph 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/clone-graph/>  

![](/assets/img/sample/leetcode/133/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/133/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- 그래프문제입니다.
- 들어온 그래프를 깊은 복사를 해서 리턴합니다.
- discuss를 보고 이해했습니다.


-----  

## 5. code


**c++**

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
    //map을 사용하는 이유는 모든 경로를 조사해서 값을 넣는다해도 중복값이 저장되지 않기 때문입니다.
    private:
    unordered_map<Node*,Node*> um;
public:
    Node* cloneGraph(Node* node) {
        if(!node)return NULL;
        Node* cpnode = new Node(node->val);
        um[node] = cpnode;
        queue<Node*> nq;
        nq.push(node);
        while(!nq.empty()){
            Node* origin = nq.front();
            nq.pop();
            for(int i  = 0;i<origin->neighbors.size();++i){
                Node* neighbor = origin->neighbors[i];
                if(um.find(neighbor) ==um.end()){
                    um[neighbor] = new Node(neighbor->val);
                    nq.push(neighbor);
                }
                um[origin]->neighbors.push_back(um[neighbor]);
            }
        }
        return cpnode;
    }
};

```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

 






