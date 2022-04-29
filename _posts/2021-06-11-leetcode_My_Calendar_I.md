---
title: leetcode(리트코드)6월10일 challenge729-My Calendar I(python)
date: 2021-06-11 00:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 10일 - My Calendar I 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/my-calendar-i/>  

![](/assets/img/sample/leetcode/729/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/729/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 10일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 도서관 대여시스템을 생각하면 됩니다. start와 end까지 누군가 빌렸다고 생각하고 새로운 start와 end가 들어왔을 때 이미 빌린 시점이라면 False 아니라면 True를 리턴합니다.




-----  

## 5. code


**python**

```python

class MyCalendar:

    def __init__(self):
        self.dq = []

    def book(self, start: int, end: int) -> bool:
        for s,e in self.dq : 
            if start < e and end > s : 
                return False
        
        self.dq.append([start,end])
        return True


# Your MyCalendar object will be instantiated and called as such:
# obj = MyCalendar()
# param_1 = obj.book(start,end)
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/729/result.JPG)  

필요시 c++로 풀어드립니다.



