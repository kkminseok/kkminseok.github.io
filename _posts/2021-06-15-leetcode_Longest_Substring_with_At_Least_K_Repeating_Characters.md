---
title: leetcode(리트코드)-395 Longest Substring with At Least K Repeating Characters(python)
author: 강민석
date: 2021-06-15 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 175 - Longest Substring with At Least K Repeating Characters 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/> 

![](/assets/img/sample/leetcode/395/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/395/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 부분 문자열에서 모든 문자들이 k만큼 반복되거나 커야합니다.
- 해당 부분 문자열 중에서 가장 긴 값을 리턴하세요.




-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def longestSubstring(self, s: str, k: int) -> int:      
        if len(s) == 0 or k > len(s) :
            return 0
        if k ==0 :
            return len(s)
        
        #기존 코드 효율성 있게 바꾸기.
        '''
        dic = {}
        for i in range(len(s)):
            if s[i] not in dic : 
                dic[s[i]] = 1
            else:
                dic[s[i]] += 1
        idx = 0
        while idx < len(s) and dic[s[idx]] >=k :
            idx+=1
        '''
        idx = 0
        while idx < len(s) and s.count(s[idx]) >=k :
            idx+=1
        if idx == len(s):
            return len(s)
        left = self.longestSubstring(s[:idx],k)
        right = self.longestSubstring(s[idx+1:],k)
            
        return max(left,right)
    
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/395/result.JPG)  

필요시 c++로 짜드리겠습니다.



