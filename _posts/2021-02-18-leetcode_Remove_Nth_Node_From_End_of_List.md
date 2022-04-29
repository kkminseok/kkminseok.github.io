---
title: leetcode(리트코드)19-Remove Nth Node From End of List
author: 강민석
date: 2021-02-18 13:30:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 19 - Remove Nth Node From End of List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-nth-node-from-end-of-list/>  

![](/assets/img/sample/leetcode/19/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/19/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- n이 주어집니다. 끝에서 n번째에 있는 노드를 지워야합니다. 한 번에 처리할 수 있게 로직을 짜야합니다.  




-----  

## 5. code

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:    
    void solution(ListNode*& first,ListNode*& tempnode,ListNode*& head,int count,int n)
    {
        if(first->next!=nullptr)
        {
            //count가 n에 도달했으면 그때부터 tempnode를 움직여줍니다.
            if(count==n)
            {
                tempnode=tempnode->next;
            }
            else
                ++count;
            solution(first->next,tempnode,head,count,n);
        }
        else
        {
            //만약 다 돌았는데 count가 n만큼 채워지지 않았다면 head를 바꿔야합니다.
            if(count!=n)
            {
                head= head->next;
            }
            else
                tempnode->next=tempnode->next->next;
        }
    }
    
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //first는 리스트의 끝까지 돌 것입니다.
        ListNode* first = head;
        //tempnode는 first와 n만큼 뒤에있는 노드입니다.
        ListNode* tempnode = head;
        solution(first,tempnode,head,0,n);
        return head; 
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(83%)**  

![](/assets/img/sample/leetcode/19/result.JPG)

**0ms코드(100%)**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int key) 
    {
         
         ListNode *first = head; 
  
        // Second pointer will point to the 
        // Nth node from the beginning 
        ListNode *second = head; 
        for (int i = 0; i < key; i++) 
        { 
  
            // If count of nodes in the given 
            // linked list is <= N 
            //리스트의 크기보다 1만큼 작은 n이 들어왔을 때
            if (second->next == NULL)  
            { 
  
                // If count = N i.e. 
                // delete the head node 
                if (i == key - 1) 
                    head = head->next; 
                return head; 
            } 
            second = second->next; 
        } 
  
        // Increment both the pointers by one until 
        // second pointer reaches the end 
        while (second->next != NULL) 
        { 
            first = first->next; 
            second = second->next; 
        } 
  
        // First must be pointing to the 
        // Nth node from the end by now 
        // So, delete the node first is pointing to 
        first->next = first->next->next; 
        return head; 
        
        
    }
};
```

