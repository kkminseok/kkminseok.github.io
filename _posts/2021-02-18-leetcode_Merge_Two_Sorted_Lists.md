---
title: leetcode(리트코드)21-Merge Two Sorted Lists
author: 강민석
date: 2021-02-18 15:10:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 21 - Merge Two Sorted Lists 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/merge-two-sorted-lists/>  

![](/assets/img/sample/leetcode/21/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/21/input.JPG)  

**Constraints:**

- The number of nodes in both lists is in the range [0, 50].
- -100 <= Node.val <= 100
- Both l1 and l2 are sorted in non-decreasing order.

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 두 리스트를 합병정렬 합니다.  

-----  

## 5. code

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* result = new ListNode();
        ListNode* head = result;
        while(l1!=nullptr && l2!=nullptr)
        {
            if(l1->val >= l2->val)
            {
                result->next = l2;
                l2=l2->next;
            }
            else
            {
                result->next=l1;
                l1=l1->next;
            }
                result= result->next;
        }
        while(l1!=nullptr)
        {
            result->next=l1;
            result=result->next;
            l1=l1->next;
        }
        while(l2!=nullptr)
        {
            result->next=l2;
            result=result->next;
            l2=l2->next;
        }
        
        return head->next;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(81%)**  

![](/assets/img/sample/leetcode/21/result.JPG)

**시간0ms(100%) 코드**

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        //임시 노드
        ListNode dummy(-3);
        //그것을 가리키는 꼬리노드
        ListNode *tail = &dummy;
        //l1과 l2가 nullptr이 아니면
        while (l1 and l2) {
            if (l1->val < l2->val) {
                tail->next = l1;
                l1 = l1->next;
            } else {
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }
        //끝난 것을 기준으로 다른 노드를 아예 갖다 붙임.
        tail->next = l1 ? l1 : l2;
        return dummy.next;
        
        
    }
};
```

**기존 내 코드에서 개선한 코드 0ms 100%**

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* result = new ListNode();
        ListNode* head = result;
        while(l1!=nullptr && l2!=nullptr)
        {
            if(l1->val >= l2->val)
            {
                result->next = l2;
                l2=l2->next;
            }
            else
            {
                result->next=l1;
                l1=l1->next;
            }
                result= result->next;
        }
        //이 부분
        result->next = l1 ? l1 : l2;
        
        return head->next;
    }
};
```