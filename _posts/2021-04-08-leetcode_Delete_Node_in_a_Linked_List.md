---
title: leetcode(리트코드)237-Delete Node in a Linked List
author: 강민석
date: 2021-04-08 00:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 237 - Delete Node in a Linked List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/delete-node-in-a-linked-list/>  

![](/assets/img/sample/leetcode/237/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/237/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 들어온 노드 값을 제거합니다. 완벽하게 제거하지 않아도 accept가 되기에 좋은 문제는 아닙니다.

-----  

## 5. code


**c++**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 80% python 57%** 
![](/assets/img/sample/leetcode/237/result.JPG)  






