---
title: leetcode(리트코드)-1624 Largest Substring Between Two Equal Character(python)
author: 강민석
date: 2021-05-23 04:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1624 - Largest Substring Between Two Equal Character 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/largest-substring-between-two-equal-characters/> 

![](/assets/img/sample/leetcode/1624/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1624/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 직관적인 문제입니다. 문자가 중복되었을 때 그 사이에있는 문자열의 크기가 제일 큰 값을 리턴합니다.


-----  

## 5. code  

코드설명

dictionary 자료형에 값이 이미 들어와있으면 중복된 것이므로 현재 인덱스에서 dic에 저장된 최초의 인덱스를 빼서 큰 값을 갱신해줍니다.

**python**

```python
class Solution:
    def maxLengthBetweenEqualCharacters(self, s: str) -> int:
        dic = {}
        ans = -1
        for i in range(len(s)):
            if s[i] not in dic:
                dic[s[i]] = i
            else : 
                ans = max(ans,i - dic[s[i]] - 1) 
        return ans
```



-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1624/result.JPG)  

필요시 c++로 짜드리겠습니다.



