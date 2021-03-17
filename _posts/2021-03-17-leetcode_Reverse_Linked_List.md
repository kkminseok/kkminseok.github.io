---
title: leetcode(리트코드)206-Reverse Linked List
author: 강민석
date: 2021-03-17 01:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 206 - Reverse Linked List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reverse-linked-list/>  

![](/assets/img/sample/leetcode/206/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/206/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- LinkedList가 주어집니다. 뒤집어야합니다.

-----  

## 5. code

**c++**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    void save(ListNode* head,vector<int>& vec)
    {
        if(head!=nullptr)
        {

            vec.push_back(head->val);
            save(head->next,vec);
        }
        
    }
    //재귀
    void put(ListNode* node,vector<int>& vec,int count)
    {
        if(node!=nullptr)
        {
            node->val = vec[count--];
            put(node->next,vec,count);
        }
    }
    //for문
    void put2(ListNode* node,vector<int>& vec,int count)
    {
        for(;count>=0;--count)
        {
            node->val = vec[count];
            node=node->next;
        }
    }
    ListNode* reverseList(ListNode* head) {
        ListNode* savenode = head;
        vector<int> vec;
        save(savenode,vec);
        savenode= head;
        //put(head,vec,vec.size()-1);
        //put2(head,vec,vec.size()-1);
        //수정 구문
        ListNode* prev= nullptr;
        ListNode* next;
        while(savenode!=nullptr)
        {
            next = savenode->next;
            savenode->next = prev;
            prev = savenode;
            savenode=next;
        }
        
        return prev;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 72%** 
![](/assets/img/sample/leetcode/206/result.JPG)  

코드를 개선하였으나 시간차이가 별로 없었다..

아이디어만 배워갔다.



