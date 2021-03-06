---
title: leetcode(리트코드)23-Merge K Sorted Lists
author: 강민석
date: 2021-02-21 12:10:00 +0800
categories: [leetcode,Hard]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 23 - Merge K Sorted Lists 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/merge-k-sorted-lists/>  

![](/assets/img/sample/leetcode/23/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/23/input.JPG)  


**Constraints:**

- k == lists.length
- 0 <= k <= 10^4
- 0 <= lists[i].length <= 500
- -10^4 <= lists[i][j] <= 10^4
- lists[i] is sorted in ascending order.
- The sum of lists[i].length won't exceed 10^4.

-----  

## 3. 분류 및 난이도

Hard 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- K로 주어진 리스트들의 벡터를 정렬해야합니다.

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
    ListNode* TwoListMerger(ListNode* list1,ListNode* list2)
    {
        if(list1 ==nullptr)
            return list2;
        else if(list2==nullptr)
            return list1;
        if(list1->val <=list2->val)
        {
            list1->next = TwoListMerger(list1->next,list2);
            return list1;
        }
        else
        {
            list2->next=TwoListMerger(list1,list2->next);
            return list2;
        }
        
    }
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.empty())
            return nullptr;
        while(lists.size()>1)
        {
            lists.push_back(TwoListMerger(lists[0],lists[1]));
            lists.erase(lists.begin());
            lists.erase(lists.begin());
        }
        return lists.front();
       
    }
};
```


-----

## 6. 결과 및 후기, 개선점

Discuss 참조.


**0ms 100% 코드**

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        map<int, int> counts;
        ListNode *head, *aux;
        //더미노드를 가리키는 헤드
        head = new ListNode();
        aux = head;
        
        //list를 돌면서 map에 벡터요소에 대한 카운트값을 증가시켜줍니다.
        for (int i = 0; i < lists.size(); i++) {
            ListNode *p = lists[i];
            while (p != nullptr) {
                counts[p->val]++;
                p = p->next;
            }
        }
        //map을 돌면서 노드들을 연결합니다.
        for (auto it = counts.begin(); it != counts.end(); it++) {
            for (int i = 0; i < it->second; i++) {
                aux->next = new ListNode(it->first);
                aux = aux->next;
            }
        }
        
        aux = head;
        head = head->next;
        delete aux;
        
        return head;
    }
};
```
