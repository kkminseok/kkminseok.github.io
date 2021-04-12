---
title: leetcode(리트코드)4월12일 challenge667-Beautiful ArrangeMent II
author: 강민석
date: 2021-04-12 10:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 12일 - Beautiful ArrangeMent II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/beautiful-arrangement-ii/>  

![](/assets/img/sample/leetcode/667/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/667/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 12일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 1 ~ n의 숫자를 가지는 배열이 있습니다.
- 각 배열의 인덱스의 차이의 절대값이 k만큼 다른 배열을 리턴합니다.





-----  

## 5. code

**c++**

```c++
class Solution {
public:
    vector<int> constructArray(int n, int k) {
        vector<int> res(n,0);
        for(int i = 1 ; i< n+1;++i)
            res[i-1] = i;
        for(int i = 1 ; i < k ;++i){
            int left = i;
            int right= n-1;
            while(left<right){
                swap(res[left++],res[right--]);
            }
        }
        return res;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/667/result.JPG)  




