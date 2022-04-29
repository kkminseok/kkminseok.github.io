---
title: leetcode(리트코드)5월21일 challenge890-Find and Replace Pattern
date: 2021-05-21 05:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 21일 - Find and Replace Pattern 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-and-replace-pattern/>  

![](/assets/img/sample/leetcode/890/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/890/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 21일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- pattern이 주어지고, words가 주어집니다. pattern과 동일하게 반복되는 문자열들을 찾아 결과로 리턴합니다.

- python은 ASCII로 변환하기 까다롭기에, dictionary 자료형을 사용해야한다는 것을 알았습니다.

- discuss를 보던 중 zip()함수에 대해 알게 되었습니다.
    + "abc", "add" 라는 문자열이 zip의 인자로 들어오면 ["a,a"],["b,d"],["c,d"]로 묶어줍니다.



-----  

## 5. code


**python**

```python
class Solution:
    def findAndReplacePattern(self, words: List[str], pattern: str) -> List[str]:
        res =[]
        
        def match(word,pattern):
            if len(word) != len(pattern):
                return False
            dir ={}
            for w,p in zip(word,pattern) :
                # 만약 dir에 처음 들어온 값인데
                if w not in dir :
                    # 이미 p는 어디간에 존재하면
                    if p in dir.values():
                        return False
                    dir[w]= p
                else : 
                    # 만약 먼저 들어온 값과 p가 다르면
                    if dir[w] != p :
                        return False
            return True
            
        for w in words : 
            if match(w,pattern) : 
                res.append(w)
        return res
        
```



-----

## 6. 결과 및 후기, 개선점

필요시 c++로 풀어드립니다.
zip()에 대해 알게되었다.



