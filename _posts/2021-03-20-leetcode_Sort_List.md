---
title: leetcode(리트코드)148-Sort List
author: 강민석
date: 2021-03-19 05:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 148 - Sort List 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sort-list/>  

![](/assets/img/sample/leetcode/148/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/148/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- LinkedList가 주어집니다. 정렬하세요.

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
    ListNode* sortList(ListNode* head) {
        vector<int> temp;
        ListNode* result = head;
        while(result!=nullptr)
        {
            temp.push_back(result->val);
            result=result->next;
        }
        sort(temp.begin(),temp.end());
        result = head;
        for(size_t i =0;i<temp.size();++i)
        {
            result->val = temp[i];
            result = result->next;
        }
        
        return head;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 95% python ??%** 
![](/assets/img/sample/leetcode/148/result.JPG)  


문제에서는 nlogn 으로 정렬하라 했기에 해당 코드를 가져왔습니다.

```c++
class Solution {
public:
	ListNode *sortList(ListNode *head) {
		if(!head || !(head->next)) return head;
		
		//linked list의 길이를 구합니다.
		ListNode* cur = head;
		int length = 0;
		while(cur){
			length++;
			cur = cur->next;
		}
		//더미노드
		ListNode dummy(0);
		dummy.next = head;
		ListNode *left, *right, *tail;
		for(int step = 1; step < length; step <<= 1){
			cur = dummy.next;
			tail = &dummy;
			while(cur){
				left = cur;
				right = split(left, step);
				cur = split(right,step);
				tail = merge(left, right, tail);
			}
		}
		return dummy.next;
	}
private:
	/**
	 처음 들어온 head 노드를 기준으로 n만큼 이동해서 잘라버립니다. 잘라버린 노드는 second이고 그것을 반환하여 main method의 right,cur이 됩니다.
	 */
	ListNode* split(ListNode *head, int n){
		//if(!head) return NULL;
		for(int i = 1; head && i < n; i++) head = head->next;
		
		if(!head) return NULL;
		ListNode *second = head->next;
		head->next = NULL;
		return second;
	}
	/**
	  * merge the two sorted linked list l1 and l2,
	  * then append the merged sorted linked list to the node head
	  * return the tail of the merged sorted linked list
	 */
	ListNode* merge(ListNode* l1, ListNode* l2, ListNode* head){
		ListNode *cur = head;
		while(l1 && l2){
			if(l1->val > l2->val){
				cur->next = l2;
				cur = l2;
				l2 = l2->next;
			}
			else{
				cur->next = l1;
				cur = l1;
				l1 = l1->next;
			}
		}
		cur->next = (l1 ? l1 : l2);
		while(cur->next) cur = cur->next;
		return cur;
	}
};
```