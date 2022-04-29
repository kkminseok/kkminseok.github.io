---
title: leetcode(리트코드)-677 Map Sum Pairs(PYTHON)
author: 강민석
date: 2021-07-30 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 677 - Count Items Matching a Rule  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/map-sum-pairs/> 

![](/assets/img/sample/leetcode/677/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/677/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- Mapsum을 만들어야 합니다.
- insert()값으로 키와 값이 들어오는데 같은 값이 중복으로 들어올 시 해당 키에 저장된 값을 갱신해주면 됩니다.
- sum()값으로는 문자열이 들어옵니다.
    - prefix로 문자열 맨 앞에 나타나야 합니다.
    - sum() 함수를 호출하면 해당 prefix를 가진 키들의 값을 합해 리턴합니다.


-----  

## 5. code  

코드설명


**python**

```python
class MapSum:

    def __init__(self):
        self.dic = {}

    def insert(self, key: str, val: int) -> None:
            self.dic[key] = val

    def sum(self, prefix: str) -> int:
        res = 0 
        for word in self.dic : 
            idx = word.find(prefix)
            if idx == 0 :
                res += self.dic[word]
        return res       
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/677/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


