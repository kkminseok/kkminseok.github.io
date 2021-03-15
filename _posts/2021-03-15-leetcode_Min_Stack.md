---
title: leetcode(리트코드)155-Min Stack
author: 강민석
date: 2021-03-15 00:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 155 - Min Stack 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/min-stack/>  

![](/assets/img/sample/leetcode/155/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/155/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- stack을 구현해야합니다. getMin()함수를 호출하면 요소중 가장 작은 값을 리턴해야합니다.

-----  

## 5. code

**python**

```python
class MinStack:

    def __init__(self):
        self.st = []
        """
        initialize your data structure here.
        """
        

    def push(self, x: int) -> None:
        currmin = self.getMin()
        
        if currmin == None or x < currmin : 
            currmin = x
        self.st.append((x,currmin))
        

    def pop(self) -> None:
        self.st.pop()

    def top(self) -> int:
        if len(self.st) == 0 :
            return None
        return self.st[len(self.st) -1][0]

    def getMin(self) -> int:
        if len(self.st) == 0 :
            return None
        return self.st[len(self.st)-1][1]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

-----  

**c++**

```c++
class MinStack {
private:
    multiset<int> ms;
    stack<int> st;
public:
    /** initialize your data structure here. */
    MinStack() {
    }
    void push(int x) {
        st.push(x);
        ms.insert(x);
    }
    
    void pop() {
        int temp = st.top();
        st.pop();
        auto findindex = ms.find(temp);
        if(findindex!=ms.end())
        {
            ms.erase(findindex);
        }
            
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        multiset<int>::iterator iter = ms.begin();
        return *iter;
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 70% python 70%**  
![](/assets/img/sample/leetcode/155/result.JPG)  

python은 40ms 코드와 다를 게 없었고,

 
**c++99% code**

```c++
class MinStack {
public:
    
    
    stack<int> s,mn;
    
    
    MinStack() {
        
    }
    
    void push(int x) {
        s.push(x);
        //단순한 구문이지만 핵심.
        if((mn.empty())||(x<=mn.top()))
        {mn.push(x);}
    }
    
    void pop() {
        
        int nn=s.top();
        s.pop();
        
        if(nn==mn.top())
        {mn.pop();}
        
    }
    
    int top() {
        return s.top();
    }
    
    int getMin() {
        return mn.top();
    }
};
```

