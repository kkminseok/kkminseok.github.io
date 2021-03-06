---
title: leetcode(리트코드)3월04일 challenge160-Intersection of Two Linked Lists
author: 강민석
date: 2021-03-04 01:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 04일 - Intersection of Two Linked Lists 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/intersection-of-two-linked-lists/>  

![](/assets/img/sample/leetcode/160/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/160/input.JPG)  




**Notes:**

- The number of nodes of listA is in the m.
- The number of nodes of listB is in the n.
- 1 <= m, n <= 3 * 104
- 1 <= Node.val <= 105
- 1 <= skipA <= m
- 1 <= skipB <= n
- intersectVal is 0 if listA and listB do not intersect.
- intersectVal == listA[skipA + 1] == listB[skipB + 1] if listA and listB intersect.

-----  

## 3. 분류 및 난이도

eazy 난이도입니다.  
3월 04일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 두 개의 링크드 리스트에서 이어진 부분을 찾아야합니다.




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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(headA == nullptr || headB == nullptr)
            return 0;
        
        ListNode* a = headA;
        ListNode* b = headB;
        
        while(a!=b)
        {
            a = a == nullptr ? headB : a->next;
            b = b == nullptr ? headA : b->next;
        }
        return a;
    }
};
```

-----  

**python**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if headA == None or headB == None :
            return None
        
        a = headA
        b = headB
        while a != b:
            a = headB if a == None else a.next
            b = headA if b == None else b.next
        
        return a
```

-----

## 6. 결과 및 후기, 개선점

84%

![](/assets/img/sample/leetcode/160/result.JPG)  

**24ms 99% 코드**

```c++
int speedup = []() {ios::sync_with_stdio(0); cin.tie(0); return 0;}();

class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *pA = headA, *pB = headB;
        while (pA != pB) {
            pA = pA ? pA->next : headB;
            pB = pB ? pB->next : headA;
        }
        return pA;
    }
};
```

입출력 향상 부분 말고는 다른 점이 없습니다.


