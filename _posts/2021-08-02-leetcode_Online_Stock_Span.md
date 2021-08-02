---
title: leetcode(리트코드)-901 Online Stock Span(PYTHON)
author: 강민석
date: 2021-08-02 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 901 - Online Stock Span  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/online-stock-span/> 

![](/assets/img/sample/leetcode/901/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/901/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 순차적으로 들어오는 데이터에 대해 몇 번째 인덱스를 기준으로 항상 상승폭이었는 지를 리턴합니다.





-----  

## 5. code  

코드설명

- 잘 모르겠어서 discuss를 참고하여 스택으로 풀었습니다.

**python**

```python
class StockSpanner:

    def __init__(self):
        self.dq = deque()

    def next(self, price: int) -> int:
        res = 1
        while self.dq and self.dq[-1][0] <= price : 
            res += self.dq.pop()[1]
        self.dq.append((price,res))
        return res


# Your StockSpanner object will be instantiated and called as such:
# obj = StockSpanner()
# param_1 = obj.next(price)            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/901/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


