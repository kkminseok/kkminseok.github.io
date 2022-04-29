---
title: leetcode(리트코드)-1352 Product of the Last K Numbers(PYTHON)
author: 강민석
date: 2021-08-09 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1352 - Product of the Last K Numbers  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/product-of-the-last-k-numbers/> 

![](/assets/img/sample/leetcode/1352/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1352/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 구현문제입니다.
- add()함수는 리스트에 값들을 넣습니다.
- getProduct()함수는 안에 들어온 값을 기준까지 리스트 맨 뒤에서 세어서 그 값들을 곱한 값을 리턴해야합니다.



-----  

## 5. code  

코드설명

- product는 리스트입니다.
- zerobase는 가장 최근에 들어온 '0'의 위치값입니다. 추후 getproduct()를 해줬을 때 이 이 위치값을 기준으로 값을 바로 리턴해줄 것입니다.
- 일단 product리스트에 들어오는 요소를 다 곱해버립니다.
    - 만약 최근에 들어온 값이 '0'인 경우 값을 그대로 넣습니다.



**python**

```python
class ProductOfNumbers:

    def __init__(self):
        self.product = deque()
        self.zerobase = -1

    def add(self, num: int) -> None:
        if num == 0 :
            self.zerobase = len(self.product) 
        if len(self.product) == 0 :
            self.product.append(num)
        else : 
            if self.product[-1] != 0 : 
                self.product.append(num * self.product[-1])
            else : 
                self.product.append(num)

    def getProduct(self, k: int) -> int:
        idx = len(self.product) - k
        #print(self.product,idx,self.zerobase)
        if idx <= self.zerobase : 
            return 0
        if idx > 0 :
            if self.product[idx-1] == 0 :
                return self.product[-1]
            else : 
                return self.product[-1] // self.product[idx-1]
        else : 
            return self.product[-1]

# Your ProductOfNumbers object will be instantiated and called as such:
# obj = ProductOfNumbers()
# obj.add(num)
# param_2 = obj.getProduct(k)          
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1352/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


