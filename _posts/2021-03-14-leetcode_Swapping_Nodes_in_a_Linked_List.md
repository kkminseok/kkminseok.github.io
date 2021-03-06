---
title: leetcode(리트코드)3월14일 challenge1721-Swapping Nodes in a Linked List
author: 강민석
date: 2021-03-14 17:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 14일 - Swapping Nodes in a Linked List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/swapping-nodes-in-a-linked-list/>  

![](/assets/img/sample/leetcode/1721/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1721/input.JPG)  

**Constraints:**

- The number of nodes in the list is n.
- 1 <= k <= n <= 105
- 0 <= Node.val <= 100

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 14일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 앞에서 k번째의 수와 뒤에서 k 번째의 수를 바꿔주면 됩니다.





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
    void calsize(ListNode* head, int& count)
    {
        if(head->next==nullptr)
            return;
        calsize(head->next,++count);
    }
    void swappointer(ListNode* head,int count, int k,ListNode*& first,ListNode*& second,int size)
    {
        if(head==nullptr)
            return ;
        if( k== count)
        {
            first = head;
        }
        if(size - k +1== count)
        {
            second = head;
            int temp = first->val;
            first->val = second->val;
            second->val= temp;
        }
        swappointer(head->next,++count,k,first,second,size);
    }
    ListNode* swapNodes(ListNode* head, int k) {
        //첫 번째 돌 때 전체의 갯수를 알게하자.
        ListNode* tempsize = head;
        int size = 1;
        calsize(tempsize,size);
        cout<<size;
        ListNode* first;
        ListNode* second;
        //second부터 만나는 경우를 없애버림.
        if(size/2 < k)
            k = size-k+1;
        swappointer(tempsize,1,k,first,second,size);
        return head;
    
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 99%**

![](/assets/img/sample/leetcode/1721/result.JPG)  
