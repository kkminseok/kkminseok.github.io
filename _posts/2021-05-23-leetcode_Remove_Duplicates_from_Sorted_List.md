---
title: leetcode(리트코드)-83 Remove Duplicates from Sorted List(python)
author: 강민석
date: 2021-05-23 05:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 83 - Remove Duplicates from Sorted List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-duplicates-from-sorted-list/> 

![](/assets/img/sample/leetcode/83/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/83/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 직관적인 문제입니다. list가 중복되면 그 값을 거하여 1개만 존재하게 하면 됩니다.



-----  

## 5. code  

코드설명

dictionary 자료형에 값이 이미 들어와있으면 중복된 것이므로 현재 인덱스에서 dic에 저장된 최초의 인덱스를 빼서 큰 값을 갱신해줍니다.

**python**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        #결과값은 맨 앞을 가르켜야하므로
        res = head
        def solve(root):
            if root : 
                if root.next != None and root.val == root.next.val : 
                    root.next = root.next.next
                    # 1 -> 1 -> 1 같은 중복이 연속될 경우가 있습니다.
                    solve(root)
                else:
                    solve(root.next)
        solve(head)
        return res
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/83/result.JPG)  

필요시 c++로 짜드리겠습니다.



