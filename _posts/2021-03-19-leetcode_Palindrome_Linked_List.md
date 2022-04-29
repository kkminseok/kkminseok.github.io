---
title: leetcode(리트코드)234-Palindrome Linked List
author: 강민석
date: 2021-03-19 03:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 234 - Palindrome Linked List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/palindrome-linked-list/>  

![](/assets/img/sample/leetcode/234/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/234/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- Linked List가 주어집니다. 꼬리에서 출발해도 같은 값(palindrome)을 갖는 지 확인해주세요.

-----  

## 5. code


**c++**

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
    string str = "";
    void check(ListNode* head)
    {
        if(head==nullptr)
            return ;
        str += std::to_string(head->val);
        check(head->next);
    }
    
    bool isPalindrome(ListNode* head) {
        check(head);
        string temp = str;
        reverse(str.begin(),str.end());
        if(temp==str)
            return true;
        return false;
    }
};
```

**python은 discuss에서 긁어온 것**

```python
def isPalindrome(self, head):
    vals = []
    #head가 None이 아닐 때 값을 다 리스트에 넣는다.
    while head:
        vals += head.val,
        head = head.next
    #리스트의 끝부터 검사.
    return vals == vals[::-1]
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 5% python ??%** 
![](/assets/img/sample/leetcode/234/result.JPG)  




**c++ 8ms 96% code**

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        ListNode* nxt = NULL;
        while(fast && fast->next){
            fast = fast->next->next;
            ListNode* tmp = slow->next;
            slow->next = nxt;
            nxt = slow;
            slow = tmp;
        }
        if(fast) slow = slow->next;
        while(slow && nxt){
            if(slow->val != nxt->val) return false;
            slow = slow->next;
            nxt = nxt->next;
        }
        return true;
    }
};
```

nxt라는 새로운 노드를 만들어서 반대로 가는 리스트를 만듭니다.
fast가 끝까지 도달하면 slow는 반쯤 온것 이므로 위치를 조정해주고 nxt와 slow를 비교합니다.


