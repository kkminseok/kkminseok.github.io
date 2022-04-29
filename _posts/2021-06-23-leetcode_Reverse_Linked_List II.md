---
title: leetcode(리트코드)6월23일 challenge92-Reverse Linked List II(python)
date: 2021-06-23 01:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 23일 - Reverse Linked List II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reverse-linked-list-ii/>  

![](/assets/img/sample/leetcode/92/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/92/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 23일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 링크드 리스트가 주어집니다.
- left와 right 범위내의 값들의 순서를 바꿔야합니다.
- 대칭으로 바꾸면 됩니다.





-----  

## 5. code


**python**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseBetween(self, head: ListNode, left: int, right: int) -> ListNode:
        #left와 right 감지용 변수
        count = 1
        #stack을 통해 값들을 바꿔줄 것입니다.
        st = deque()
        lnode = head
        rnode =head
        # rnode는 right를 가리키는 노드인데, 그 값을 찾으면 다음 노드는 문제에서 중요하지 않으므로 탐색해주지 않습니다.
        while rnode :
            #left노드를 찾으면 right노드도 찾습니다. 찾는동안 스택에 값들을 저장합니다. 
            if count == left : 
                lnode = rnode
                while rnode and count != right+1 :  
                    count+=1
                    st.append(rnode.val)
                    rnode = rnode.next
                break
            else : 
                rnode = rnode.next
                count+=1
        #스택이 없어질 때까지 값들을 넣으면 완성.            
        while st : 
            lnode.val = st.pop()
            lnode = lnode.next
        return head
        
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/92/result.JPG)  

필요시 c++로 풀어드립니다.



