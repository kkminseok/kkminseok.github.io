---
title: leetcode(리트코드)-203 Remove Linked List Elements(python)
author: 강민석
date: 2021-06-16 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 203 - Remove Linked List Elements 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-linked-list-elements/> 

![](/assets/img/sample/leetcode/203/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/203/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 링크드 리스트가 주어지는데, val로 들어온 값을 제거한 링크드 리스트를 리턴하세요.




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
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        def solve(head,val):
            #범위를 벗어나지 않게.
            if head and head.next : 
                if head.next.val == val :
                    if head.next.next == None :
                        head.next = None
                    else:
                        head.next = head.next.next
                    solve(head,val)
                else : 
                    solve(head.next,val)
        #처음 들어온 값들이 val과 같은 값인 경우.
        while head and head.val == val : 
            head = head.next
        res = head
        solve(head,val)
        return res
    
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/203/result.JPG)  

필요시 c++로 짜드리겠습니다.



