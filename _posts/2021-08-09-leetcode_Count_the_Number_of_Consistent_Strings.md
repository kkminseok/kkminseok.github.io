---
title: leetcode(리트코드)-1684 Count the Number of Consistent Strings(PYTHON)
author: 강민석
date: 2021-08-08 02:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1684 - Count the Number of Consistent Strings  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-the-number-of-consistent-strings/> 

![](/assets/img/sample/leetcode/1684/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1684/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- allowed와 words가 주어집니다.
- allowed에 들어있는 문자들은 '같은 취급합니다.'
- 같은 취급을 하였을 때 words에 들어있는 word들에 대해서 연속된 문자열의 갯수를 구하세요 allowed가 'ab'이고 word가 'ab'여도 같은 취급을 하는 것입니다.



-----  

## 5. code  

코드설명


- 좋지 않은 코드입니다. 반대로 특정 문자가 word안에 없으면 그 즉시 반복문을 깨는 코드로 작성하는 편이 좋습니다.


**python**

```python
class Solution:
    def countConsistentStrings(self, allowed: str, words: List[str]) -> int:
        ans = 0
        for i in range(len(words)):
            res = 0 
            size = len(words[i])
            for j in range(len(allowed)):
                res += words[i].count(allowed[j])
            if res == size : 
                ans += 1
        return ans
            

'''best code
        count = 0
        for word in words:
            for w in word:
                if not w in allowed:
                    count += 1
                    break
        return len(words)-count

'''

```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1684/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


