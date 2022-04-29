---
title: leetcode(리트코드)-1913 Maximum Product Difference Between Two Pairs(python)
author: 강민석
date: 2021-06-30 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1913 - Maximum Product Difference Between Two Pairs   문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-product-difference-between-two-pairs/> 

![](/assets/img/sample/leetcode/1913/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1913/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 리스트의 (a,b,c,d)를 뽑아서 (c * d) - (a * b)의 값이 최대치가 되는 값을 리턴하세요.
- 정렬해서 하면 참 쉽겠죠?





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = ""
        st  = deque()
        for i in range(len(s)):
            if st :
                if st[-1] == s[i] : 
                    st.pop()
                else : 
                    st.append(s[i])
            else : 
                st.append(s[i])
        while st : 
            res+=st.popleft()
        return res
            
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1913/result.JPG)  

필요시 c++로 짜드리겠습니다.



