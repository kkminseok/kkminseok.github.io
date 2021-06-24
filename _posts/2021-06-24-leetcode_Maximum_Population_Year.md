---
title: leetcode(리트코드)-1854 Maximum Population Year(python)
author: 강민석
date: 2021-06-24 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1854 - Maximum Population Year 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-population-year/> 

![](/assets/img/sample/leetcode/1854/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1854/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- logs에 들어오는 것은 어떤 사람의 출생기록입니다. [i][0]은 출생 년도, [i][1]은 사망연도 입니다.
- 사망연도는 인구수에 포함시키지 않을 때 인구수가 가장 많았을 때의 연도를 리턴하세요.
- Eazy문제라서 만만하게 봤는데, 생각보다 빠르게 처리가 불가능해서 하드코딩으로 작성하였습니다.
- 그래도 96% 나오는 거 보니 하드코딩해서 푸는게 맞는 것 같습니다.





-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def maximumPopulation(self, logs: List[List[int]]) -> int:
        #Hardcoding
        countinglist = [0] * 150
        for i in range(len(logs)):
            for j in range(logs[i][0], logs[i][1]):
                countinglist[j-1950] +=1
        #카운트가 가장 많이 쌓인 인덱스를 반환하여 + 1950을 해줍니다.
        return countinglist.index(max(countinglist)) + 1950
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1854/result.JPG)  

필요시 c++로 짜드리겠습니다.



