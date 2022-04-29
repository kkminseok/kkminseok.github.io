---
title: leetcode(리트코드)-1897 Redistribute Characters to Make All Strings Equal(python)
author: 강민석
date: 2021-06-20 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1897 - Redistribute Characters to Make All Strings Equal 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/redistribute-characters-to-make-all-strings-equal/> 

![](/assets/img/sample/leetcode/1897/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1897/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 문자열을 가진 배열이 들어옵니다. 어느 문자열로 골라도 다른 문자열과 같게 만들어야합니다. 에제에서 "aabc" 와 "bc"는 "abc"와 같게 만드려면 첫 문자열의 'a'를 "bc"의 앞에 가져다 놓으면 배열의 모든 문자열이 같게 됩니다.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def makeEqual(self, words: List[str]) -> bool:
        chrli = [0] * 26
        maxnum = 0
        size = len(words)
        if size == 1 : 
            return True
        for i in range(size):
            for j in range(len(words[i])):
                chrli[ord(words[i][j]) - 97]+=1
        for i in range(len(chrli)):
            if chrli[i] != 0 and chrli[i] % size !=0  : 
                return False
        return True
    
    
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1897/result.JPG)  

필요시 c++로 짜드리겠습니다.



