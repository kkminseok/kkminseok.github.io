---
title: leetcode(리트코드)-791 Custom Sort String(PYTHON)
author: 강민석
date: 2021-07-14 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 791 - Custom Sort String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/custom-sort-string/> 

![](/assets/img/sample/leetcode/791/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/791/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- order와 str이 주어집니다.
- order는 정렬을 할 때의 우선순위이고, str을 order의 우선순위에 맞게 재정렬하면 됩니다.

-----  

## 5. code  

코드설명
- 먼저, 결과 string에 우선순위의 카운트에 맞게 문자들을 더해줍니다.
- str을 돌면서 dic에 저장되지 않은 문자는 우선순위가 중요하지 않으므로 그대로 res string에 추가합니다.

**python**

```python
class Solution:
    def customSortString(self, order: str, str: str) -> str:
        counting = 0 
        res = ""
        dic = {}
        for word in (order):
            res += str.count(word) * word
            dic[word] = 1
        for word in str : 
            if word in dic : 
                continue
            else : 
                res += word
        print(res)
        return res
            
            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/791/result.JPG)  





