---
title: leetcode(리트코드)142-Linked List Cycle II
author: 강민석
date: 2021-03-18 01:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 142 - Linked List Cycle II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/linked-list-cycle-ii/>  

![](/assets/img/sample/leetcode/142/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/142/input.JPG)  


**Constraints:**

- The number of the nodes in the list is in the range [0, 104].
- -105 <= Node.val <= 105
- pos is -1 or a valid index in the linked-list.
 

- Follow up: Can you solve it using O(1) (i.e. constant) memory?

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- Linked List가 사이클을 돌면 어디서 부터 시작인지 노드를 반환하고 사이클이 아니면 null을 반환합니다.
- Cycle 확인하는 법은 Linked List Cycle I을 통해 알 수 있지만 어디서부터 시작인지는 몰라 Discuss를 참고하였습니다..근데 왜 저 점화식이 성립하는 지는 공부를 더 해야 알 것 같습니다.

-----  

## 5. code

**python**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        if head ==None or head.next ==None :
            return None
        onext = head
        dnext = head
        cycle = 0
        while onext != None and dnext!=None:
            if dnext.next == None :
                return None
            onext = onext.next
            dnext = dnext.next.next
            if onext == dnext:
                cycle = 1
                break
        if not cycle :
            return None
        onext = head
        while onext != dnext : 
            onext = onext.next
            dnext = dnext.next
        return onext

    
```

------  

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

    ListNode *detectCycle(ListNode *head) {
        if (head==nullptr || head->next==nullptr) return nullptr;
        
        ListNode* onext = head;
        ListNode* dnext = head;
        bool cycle = false;
        while(onext!=nullptr && dnext!=nullptr)
        {
            onext = onext->next;
            if(dnext->next ==nullptr) return nullptr;
            dnext = dnext->next->next;
            if(dnext == onext)
            {
                cycle = true;
                break;
            }
        }
        if(!cycle) return nullptr;
        
        onext = head;
        while(onext!=dnext)
        {
            onext = onext->next;
            dnext = dnext->next;
        }
        return onext;
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!


![](/assets/img/sample/leetcode/142/result.JPG)  



