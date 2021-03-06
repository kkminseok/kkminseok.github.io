---
title: leetcode(리트코드)2월27일 challenge29-Divide Two Integers
author: 강민석
date: 2021-02-27 15:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February 29 - Divide Two Integers 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/divide-two-integers/>  

![](/assets/img/sample/leetcode/29/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/29/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월27일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 버림을 사용해서 나눗셈을 하면 됩니다.
- challenge문제들은 보통 Liked 갯수가 많은 문제가 많았는데..반대의 경우는 처음 봤습니다.
- 이유인 즉슨, 오버플로 같은 처리에서 Wrong Answer 처리를 해서 그런것 같습니다. 창의성의 문제라기 보다는 정답에 끼워맞춰야하는 문제.. 예를 들어서 2^31-1 과 -1 이 들어오면 INT_MAX를 뱉어야합니다. 이유는 모르겠습니다. 

-----  

## 5. code

```c++
class Solution {
public:
    int divide(int dividend, int divisor) {
        if(dividend == INT_MIN && divisor==-1)
            return INT_MAX;
        return trunc(dividend / divisor);
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/29/result.JPG)  

