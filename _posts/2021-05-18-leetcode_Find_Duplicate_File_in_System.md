---
title: leetcode(리트코드)5월18일 challenge609-Find Duplicate File in System
date: 2021-05-18 03:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 18일 - Find Duplicate File in System 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-duplicate-file-in-system/>  

![](/assets/img/sample/leetcode/609/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/609/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 15일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- paths가 주어집니다. "()" 안에 있는 것을 기준으로 주어진 값들을 나눠 리턴해야 합니다.
- 예를 들어서 "~1.txt(abcd), ~3.txt(abcd)"인 경우 같은 abcd이므로 묶고, 마찬가지로 예제의 (efgh)도 위와같은 방식으로 해결하면 됩니다.
- 단, 단 하나로 묶어질 경우 결과값에 포함하지 않습니다.
- dictionary 자료형을 이용하여, 묶고 풀었습니다.




-----  

## 5. code


**python**

```python
class Solution:
    def findDuplicate(self, paths: List[str]) -> List[List[str]]:
        dic = {}
        res = []
        for i in range(len(paths)):
            direction = paths[i].split()
            #root directory
            root = direction[0]
            for j in range(1,len(direction)) : 
                temp = direction[j]
                name = root +"/" + temp[:temp.find("(")]  
                content = temp[temp.find("(")+1 : temp.find(")")]
                if content not in dic :
                    tlist = []
                    tlist.append(name)
                    dic[content] = tlist
                else : 
                    dic[content].append(name)
        for i in dic : 
            if len(dic[i])>=2 :
                res.append(dic[i])
        return res
```



-----

## 6. 결과 및 후기, 개선점

필요시 c++로 풀어드립니다.




