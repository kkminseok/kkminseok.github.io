---
title: leetcode(리트코드)-61 Rotate List(python)
author: 강민석
date: 2021-07-12 04:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 61 - Rotate List  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/rotate-list/> 

![](/assets/img/sample/leetcode/61/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/61/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- head가 주어집니다. k만큼 rotate한 link list를 반환하세요.

-----  

## 5. code  

코드설명

임시 deque list를 만들어서 값들을 담고, k를 갱신해줍니다.  
갱신한 k만큼 python내장함수인 rotate()을 이용해서 옮기고 노드들에 값들을 옮겨줍니다.

**python**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        if head == None : 
            return None
        temp = deque()
        res = head
        while head : 
            temp.append(head.val)
            head= head.next
        k = k % len(temp)
        finall = res
        temp.rotate(k)
        for i in range(len(temp)) : 
            res.val = temp[i]
            if i != len(temp)-1 : 
                res.next = ListNode()   
            res = res.next
            
        
        return finall
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/61/result.JPG)  

필요시 c++로 짜드리겠습니다.



