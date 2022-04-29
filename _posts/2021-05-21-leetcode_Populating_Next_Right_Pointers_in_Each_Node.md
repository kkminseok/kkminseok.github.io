---
title: leetcode(리트코드)-116 Populating Next Right Pointers in Each Node(python)
author: 강민석
date: 2021-05-21 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 116 - Populating Next Right Pointers in Each Node 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/populating-next-right-pointers-in-each-node/> 

![](/assets/img/sample/leetcode/116/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/116/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top Interview 문제입니다.


-----  

## 4. 문제 해석

- Node라는 자료형을 가진 트리의 root가 들어옵니다.
- 해당 트리는 완벽한 이진트리로 주어지며, level이 끝날 때마다 해당 노드의 next값에 '#'를 부여하며 level을 돌면 됩니다.


-----  

## 5. code  

코드설명


**python**

```python

"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:

            
    def connect(self, root: 'Node') -> 'Node':
        # 빈 root가 들어올 경우
        if not root :
            return None
        # BFS를 통해 해결할 것이므로
        dq = deque()
        #res는 더미노드입니다.
        res = Node()
        res2 =res
        dq.append(root)
        while dq : 
            size = len(dq)
            #dq의 사이즈만큼 돕니다.
            for i in range (size) :
                node = dq.popleft() 
                #자식들을 dq에 넣어줍니다.   
                if node.left : 
                    dq.append(node.left)
                if node.right : 
                    dq.append(node.right)
                res.next = Node(node.val)
                res = res.next
            # 마지막의 노드인 경우 이미 '#'으로 이미 지정되어 있으므로 하나의 예외처리를 해줬습니다.
            if len(dq)!= 0:
                res.next= Node("#")
                res = res.next
        
        
        return res2.next
            
        
            
```



-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/116/result.JPG)  

필요시 c++로 짜드리겠습니다.



