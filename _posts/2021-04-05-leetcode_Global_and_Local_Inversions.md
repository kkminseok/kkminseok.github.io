---
title: leetcode(리트코드)4월05일 challenge775-Global and Local Inversions
author: 강민석
date: 2021-04-05 05:00:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 05일 - Global and Local Inversions 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/global-and-local-inversions/>  

![](/assets/img/sample/leetcode/775/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/775/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 05일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 배열의 크기-1만큼의 요소값을 갖는 배열이 들어옵니다.
- Global과 local의 갯수가 다르면 false를 리턴합니다.
- A[i]가 i번째 인덱스와 1이상 차이나는 경우에 local과 global의 갯수가 달라집니다.
- A[i]가 i번째 인덱스와 같거나 1만큼 크면 갯수가 유지가 됩니다.(경우의 수로 안치므로)




-----  

## 5. code

**c++**

```c++
class Solution {
public:
    bool isIdealPermutation(vector<int>& A) {
        for(int i =0;i<A.size();++i){
            if(A[i] == i || A[i] ==i+1 || A[i]==i-1)     continue;
            else return false;
        }
        return true;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**c++ 81%**
![](/assets/img/sample/leetcode/775/result.JPG)  


