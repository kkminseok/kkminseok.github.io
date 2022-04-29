---
title: leetcode(리트코드)4월14일 challenge86-Partition List
author: 강민석
date: 2021-04-14 10:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 14일 - Partition List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/partition-list/>  

![](/assets/img/sample/leetcode/86/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/86/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 14일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 리스트와 x가 주어집니다.
- 리스트를 x보다 작은 것은 앞으로 크거나 같은것은 뒤로 보냅니다.
- 큰 것을 정렬할 때 상대적 위치는 같아야합니다.





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
    void setless(ListNode*& less,ListNode* originless,int x){
        if(originless){
            if(originless->val <x ){
                ListNode* newNode = new ListNode(originless->val);
                less->next = newNode;
                less = less->next;
            }
            setless(less,originless->next,x);
        }
        return;
    }
    void setmore(ListNode* more,ListNode* originmore,int x){
        if(originmore){
            if(originmore->val >= x){
                ListNode* newNode = new ListNode(originmore->val);
                more->next = newNode;
                more = more->next;
            }
            setmore(more,originmore->next,x);
        }
        return ;
    }
    
    ListNode* partition(ListNode* head, int x) {
        ListNode* more = head;
        ListNode* less = head;
        ListNode* res = new ListNode();
        ListNode* temp = res;
        setless(temp,less,x);
        setmore(temp,more,x);
        return res->next;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/86/result.JPG)  




