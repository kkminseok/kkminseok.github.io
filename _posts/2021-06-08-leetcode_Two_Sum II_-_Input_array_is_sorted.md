---
title: leetcode(리트코드)-167 Two Sum II Input array is sorted(python)
author: 강민석
date: 2021-06-08 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 167 - Two Sum II input array is sorted 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/> 

![](/assets/img/sample/leetcode/167/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/167/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 배열의 요소 2개를 뽑아내서 target을 만들어낼 수 있습니다.
- 그 2 요소의 인덱스를 리턴하세요.




-----  

## 5. code  

코드설명

**python**

```python

class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        dic = {}
        for i in range(len(numbers)):
            if target - numbers[i] in dic : 
                return [dic[target - numbers[i]],i+1]
            
            else:
                dic[numbers[i]] = i+1
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/167/result.JPG)  

필요시 c++로 짜드리겠습니다.



