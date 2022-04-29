---
title: leetcode(리트코드)6월14일 challenge1710-Maximum Units on a Truck(python)
date: 2021-06-14 01:01:56 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 14일 - Maximum Units on a Truck 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-units-on-a-truck/>  

![](/assets/img/sample/leetcode/1710/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1710/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
6월 14일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 리스트에 두 가지 값이 들어옵니다.
- 첫 번째 값은 박스의 갯수입니다.
- 두 번째 값은 박스안에 들어있는 unit의 갯수입니다.
- trucksize만큼 박스를 실을 때 최대가 되는 unit의 갯수를 리턴하세요.




-----  

## 5. code


**python**

```python
class Solution:
    def maximumUnits(self, boxTypes: List[List[int]], truckSize: int) -> int:
        boxTypes.sort(key = lambda x :x[1],reverse = True)
        res = 0
        for nob,noupb in boxTypes:
            if truckSize < nob : 
                res += truckSize * noupb
                return res
            else:
                res += (noupb * nob)
                truckSize -= nob
        return res
                
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1710/result.JPG)  

필요시 c++로 풀어드립니다.



