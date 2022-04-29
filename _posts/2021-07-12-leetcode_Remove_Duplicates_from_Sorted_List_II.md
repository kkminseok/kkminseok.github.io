---
title: leetcode(리트코드)-82 Remove Duplicates from Sorted List II(python)
author: 강민석
date: 2021-07-12 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 82 - Remove Duplicates from Sorted List II  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/> 

![](/assets/img/sample/leetcode/82/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/82/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Linked List가 주어집니다. 중복되는 값은 제거한 LinkeList를 리턴하세요.

-----  

## 5. code  

코드설명

한 번에 처리하려고 햇지만, 잘 되지 않아서 임시 리스트와 중복 판단을 위한 dictionary를 선언하여 관리하였습니다.



**python**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        temp = []
        dic = {}
        while head : 
            if head.val in dic : 
                dic[head.val] = 2
            else : 
                dic[head.val] = 1
            head = head.next
        for k in dic:
            if dic[k] == 2 : 
                continue
            else : 
                temp.append(k)
        temp.sort()
        res = ListNode()
        finall = res
        for i in range(len(temp)):
            res.next = ListNode(temp[i])
            res = res.next
        return finall.next
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/82/result.JPG)  

필요시 c++로 짜드리겠습니다.



