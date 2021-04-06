---
title: leetcode(리트코드)4월04일 challenge622-Design Circular Queue
author: 강민석
date: 2021-04-04 01:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 04일 - Design Circular Queue 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/design-circular-queue/>  

![](/assets/img/sample/leetcode/622/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/622/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 04일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 원형 큐를 만듭니다. 




-----  

## 5. code

**내가 푼 c++**

**c++**

```c++
class MyCircularQueue {
public:
    list<int> li;
    list<int>::iterator head;
    list<int>::iterator rear;
    int lisize; 
    MyCircularQueue(int k) {
        lisize = k;
    }
    
    bool enQueue(int value) {
        if(li.size()==lisize){
            return false;
        }
        else{
            li.push_back(value);
            if(li.size()==1){
                head = li.begin();
            }
            rear = li.end();
        }
        return true;
    }
    
    bool deQueue() {
        if(li.empty())return false;
        li.erase(head);
        if(!li.empty())
            head = li.begin();
        return true;
    }
    
    int Front() {
        return li.empty() ? -1 : li.front();
    }
    
    int Rear() {
        return li.empty() ? -1 : li.back();
    }
    
    bool isEmpty() {
        return li.size() == 0  ? true : false;
    }
    
    bool isFull() {
        return li.size() == lisize ? true : false;
    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```
-----

## 6. 결과 및 후기, 개선점

**c++ 64%**
![](/assets/img/sample/leetcode/622/result.JPG)  


