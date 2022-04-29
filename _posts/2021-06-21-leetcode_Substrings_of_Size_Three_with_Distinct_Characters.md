---
title: leetcode(리트코드)-1876 Substrings of Size Three with Distinct Characters(python)
author: 강민석
date: 2021-06-21 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1876 - Substrings of Size Three with Distinct Characters 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters/> 

![](/assets/img/sample/leetcode/1876/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1876/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 문자열 s가 들어옵니다. 크기가 3인 substring으로 나누었을 때 그 속에 반복되는 문자가 없는 문자열의 개수를 리턴하세요.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def countGoodSubstrings(self, s: str) -> int:
        ans = 0 
        for i in range(len(s) - 2) :
            temp = s[i:i+3]
            st = set()
            for j in range(3) : 
                if temp[j] in st : 
                    break
                st.add(temp[j])
                if j == 2 :
                    ans+=1
        return ans
                
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1876/result.JPG)  

필요시 c++로 짜드리겠습니다.



