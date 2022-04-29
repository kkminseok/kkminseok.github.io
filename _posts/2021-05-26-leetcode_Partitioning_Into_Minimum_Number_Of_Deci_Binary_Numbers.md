---
title: leetcode(리트코드)5월26일 challenge1689-Partitioning Into Minimum Number Of Deci Binary(python)
date: 2021-05-26 02:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 26일 - Partitioning Into Minimum Number Of Deci Binary 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/partitioning-into-minimum-number-of-deci-binary-numbers/>  

![](/assets/img/sample/leetcode/1689/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1689/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 26일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 왜 Medium난이도인지 모르겠습니다.
- 2진수들을 더해서 n의 값을 만들 때 몇 번 더해야하는 지 리턴하는 것인데..
- '00001'이런것도 되어서 사실상 가장 큰값을 리턴하면됩니다.





-----  

## 5. code


**python**

```python
class Solution:
    def minPartitions(self, n: str) -> int:
        return int(max(n))            
```



-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1689/result.JPG)  

필요시 c++로 풀어드립니다.



