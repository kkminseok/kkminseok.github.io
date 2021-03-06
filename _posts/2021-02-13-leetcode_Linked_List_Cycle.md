---
title: leetcode(리트코드)2월3일 challenge141-Linked List Cycle
author: 강민석
date: 2021-02-13 14:50:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge03 - Linked List Cycle문제입니다.**

## 1. 문제
<https://leetcode.com/problems/linked-list-cycle/>  

![](/assets/img/sample/leetcode/141/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/141/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월3일자 챌린지 문제입니다. 
leetcode를 2월 9일부터 시작해서 늦게나마 풉니다.  

-----  

## 4. 문제 해석

- 인풋으로 들어오는 링크드 리스트가 무한루프를 도는 리스트인지, 아닌지를 확인하여 return 해야합니다. 


-----  

## 5. code

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        for(int i=0;i<10001;++i)
        {
            if(head == nullptr)
                return false;
            head = head->next;
        }
        return true;
    }
};
```
-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/141/result.JPG) 


좋은 코드는 아닙니다. 
제한이 링크드 리스트의 사이즈 제한이 1만이라서 1만번의 반복이 지나면 false를 뱉도록 설계하였기에 제한이 커지면 커질수록 효율이 좋지 않은 코드라고 할 수 있습니다.  

**시간효율성이 좋은 코드(0ms)**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        // 빈 리스트가 들어온 경우
                if(head==NULL){
            return false;
        }
        // 리스트 요소가 1개이고, next가 없는 경우
        if(head->next==NULL){
            return false;
        }
        //slow는 1단계씩 fast는 2단계씩 next로 넘어가는데 같아지는 순간이 있다면 true를 return합니다.
        ListNode *slow = head;
        ListNode *fast = head->next->next;
        while(slow!=NULL && fast!=NULL){
            if(slow==fast){
                return true;
            }
            slow = slow->next;
            if(fast->next==NULL){
                return false;
            }
            fast = fast->next->next;
        }
        return false;
    }
};
```
