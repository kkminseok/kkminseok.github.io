---
title: leetcode(리트코드)7월06일 challenge1338-Reduce Array Size to The Half(python)
date: 2021-07-06 00:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode July 06일 - Reduce Array Size to The Half 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reduce-array-size-to-the-half/>  

![](/assets/img/sample/leetcode/1338/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1338/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
7월 06일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- arr가 주어집니다. 배열의 요소를 고릅니다.
- 고른 뒤 배열에서 해당 요소값들을 제거하였을 때, arr의 size가 최대한 절반이 되는 최소한의 배열의 요소 개수를 구해서 리턴합니다.





-----  

## 5. code


**python**

```python
class Solution:
    def minSetSize(self, arr: List[int]) -> int:
        dic = {}
        temp = []
        size = len(arr)
        for i in range(size):
            if arr[i] not in dic : 
                dic[arr[i]]=1
            else: 
                dic[arr[i]]+=1
        for num in dic : 
            temp.append(dic[num])
        temp.sort(reverse = True)
        res = 0 
        sumd = 0
        for i in range(len(temp)):
            if sumd < size//2 : 
                sumd += temp[i]
                res+=1
            else : 
                return res
        return 1
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1338/result.JPG)  

필요시 c++로 풀어드립니다.



