---
title: leetcode(리트코드)5월6일 challenge109-Convert Sorted List to Binary Search Tree
date: 2021-05-06 11:17:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 6일 - Convert Sorted List to Binary Search Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/>  

![](/assets/img/sample/leetcode/109/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/109/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 6일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 오름차순으로 정렬된 List가 들어옵니다.
- 해당 리스트를 통해 높이가 균등한 Binary Tree를 제작합니다.
- Discuss를 참조하였습니다.





-----  

## 5. code


핵심 코드는 List의 중간 노드를 찾아서 root노드로 만들어 left와 right에 붙여주는 것을 반복하는 것입니다.

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
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* BST(ListNode* head, ListNode* tail){
        ListNode* fast = head;
        ListNode* slow = head;
        if(head == tail) return nullptr;
        
        while(fast != tail && fast->next !=tail){
            fast = fast->next->next;
            slow = slow->next;
        }
        TreeNode* root = new TreeNode(slow->val);
        root->left = BST(head,slow);
        root->right = BST(slow->next,tail);
        
        return root;
    }
    
    TreeNode* sortedListToBST(ListNode* head) {
        if(head==nullptr) return nullptr;
        return BST(head,nullptr);
    }
};
//retry plz
```

-----

## 6. 결과 및 후기, 개선점

discuss

![](/assets/img/sample/leetcode/109/result.JPG)  




