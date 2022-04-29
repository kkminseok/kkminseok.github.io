---
title: leetcode(리트코드)-25 Reverse Nodes in k Group(PYTHON)
author: 강민석
date: 2021-07-18 02:34:50 +0800
categories: [leetcode,Hard]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 25 - Reverse Nodes in k Group  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reverse-nodes-in-k-group/> 

![](/assets/img/sample/leetcode/25/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/25/input.JPG)  


-----  

## 3. 분류 및 난이도

Hard 난이도 문제입니다.  


-----  

## 4. 문제 해석

- head와 k가 주어집니다.
- k만큼 그룹을 묶고 그 그룹에 해당하는 값들의 순서를 바꿔버립니다.

-----  

## 5. code  

코드설명

**python**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        #list에 넣고 돌리자.
        if k == 1 : 
            return head
        node = ListNode()
        dq = []
        res = []
        while head : 
            dq.append(head.val)
            head = head.next
        idx = 0
        while idx+k <= len(dq) : 
            temp = dq[idx:idx+ k]
            temp.reverse()
            res += temp
            idx = idx + k
        res += dq[idx:]
        temp = node
        for i in range(len(res)):
            temp.next = ListNode(res[i])
            temp = temp.next
        return node.next
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/25/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


