---
title: leetcode(리트코드)-24 Swap Nodes in Pairs(python)
author: 강민석
date: 2021-07-12 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 24 - Swap Nodes in pairs  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/swap-nodes-in-pairs/> 

![](/assets/img/sample/leetcode/24/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/24/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- head가 주어집니다. 해당 링크드 리스트에서 인접한 2개는 swap을 하여 리스트를 반환하세요.

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
    def swapPairs(self, head: ListNode) -> ListNode:
        res = head
        if head == None or head.next == None : 
            return res
        curr = head
        fast = head.next
        while True:
            curr.val, fast.val = fast.val, curr.val
            if curr.next == None or curr.next.next == None or fast.next == None or fast.next.next ==None:
                break
            curr = curr.next.next
            fast = fast.next.next
        return res
        
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/24/result.JPG)  

필요시 c++로 짜드리겠습니다.



