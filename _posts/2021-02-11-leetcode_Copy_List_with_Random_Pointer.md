---
title: leetcode(리트코드)2월10일 challenge138-Copy List with Random PointerGreater Tree
author: 강민석
date: 2021-02-11 12:02:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,LinkedList]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge10 - Copy List with Random 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/copy-list-with-random-pointer/>  

![](/assets/img/sample/leetcode/138/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/138/input.JPG)  

**Constraints:**

- 0 <= n <= 1000
- -10000 <= Node.val <= 10000
- Node.random is null or is pointing to some node in the linked list.  

-----  

## 3. 분류 및 난이도

Medium의 난이도입니다.  
2월10일자 챌린지 문제입니다.   
LinkedList문제입니다.  

-----  

## 4. 문제 해석

- 문제는 간단합니다. input으로 주어지는 링크드 리스트와 똑같은 링크드 리스트를 만드는 것입니다.
- 다만 같은 주소값을 참조해선 안됩니다. 새로운 노드들을 생성하여 연결시켜줘야합니다.
- 그러기 위해서는 원본 리스트를 도는 temp Node와 복사본 리스트를 도는 location Node를 두어 관리하였습니다.  
- 이 문제의 묘미는 random 이라는 노드인데, 원본노드에서 가리키는 random Node 또한 복사시켜줘야합니다.
    + 주소값을 계산하여 풀까 하다가 최대 1천의 크기의 리스트이므로 맨앞에서부터 random 노드와 같은 주소값을 가진 노드를 찾아 반환하였습니다.




-----  

## 5. code

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    int findrandom(Node* temp,Node* original)
    {
        if(temp->random==nullptr)   
            return -1;// null
        else
        {
            int result=0;

            while(original != temp->random)
            {
                ++result;
                original=original->next;
            }
            return result;
        }
        
    }
    Node* copyRandomList(Node* head) {
        //새헤드 노드를 만듦.        
        Node* newHead;
        //헤드노드가 null로 들어올 경우
        if(head==nullptr)
            return  nullptr;
        else
        {
            //새 헤드 노드를 원본 head노드를 참조하여 만드는 과정
            newHead = new Node(head->val);
            newHead->next=nullptr;
            newHead->random=nullptr;

            //location은 복사본 노드, tempNode는 원본 노드를 돕니다.
            Node* location = newHead;
            Node* tempNode= head;
            //다음 노드가 있으면 돕니다.
            if(tempNode->next!=nullptr)
                tempNode=tempNode->next;
            //head의 다음값이 없을 경우 while문을 안돌게 합니다.
            while(tempNode!=nullptr && head->next!=nullptr)
            {
                //일단 val값만 복사해서 넣어줍니다.
                Node* newNode = new Node(tempNode->val);
                newNode -> next = nullptr;
                newNode -> random = nullptr;
                location->next = newNode;
                tempNode = tempNode->next;
                location = location->next;
            }
            //다시 돌면서 random에 맞게
            location = newHead;
            tempNode=head;
            // random을 나중에 처리해주는 이유는 이미 생선된 리스트에서 접근을해야하므로 random을 나중에 처리한 것입니다.
            while(tempNode!=nullptr)
            {
                //원본 노드에서 가리키는 random값의 index를 찾습니다.
                int indexNum = findrandom(tempNode,head);
                //null을 가리키면 -1을 리턴하여 nullptr을 넣습니다.
                if(indexNum==-1)
                    location->random = nullptr;
                else
                {
                    Node* find = newHead;
                    for(int i=0;i<indexNum;++i)
                    {
                        find=find->next;
                    }
                    location->random = find;
                }
                
                tempNode= tempNode->next;
                location=location->next;
            }
                
            
        }
        return newHead;
    }
};
```
-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/138/result.JPG) 


**시간효율성이 좋은 코드**

제 코드와 같습니다. (99%) 
  
**공간 효율성이 좋은 코드** 

위의 코드와 같아서 따로 적진 않겠습니다.  (96%)
