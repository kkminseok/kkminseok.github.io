---
title: leetcode(리트코드)905-Sort Array By Parity    
author: 강민석
date: 2021-02-12 14:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Sort Array By Parity 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sort-array-by-parity/>  

![](/assets/img/sample/leetcode/905/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/905/input.JPG)

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 방금 전의 포스팅과 비슷한 문제로 짝수인 값만 앞으로 보내줘 배열을 리턴하는 문제입니다.  

-----  

## 5. code

```c++
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& A) {
        for(size_t i=0, j = 0;i<A.size();++i)
        {
            if(A[i]%2 ==0)
            {
                swap(A[i],A[j++]);
            }
        }
        return A;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/905/result.JPG)  

바로 전의 문제와 비슷해서 전 코드를 참고하였습니다.  

제 코드보다 시간을 반으로 줄인 코드가 있는 것 같아서 올립니다. 제코드는 4ms로 99%정도되지만 더 좋은 코드가 있네요.  

**시간 효율성이 좋은 코드**

```c++
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& A) {
        int l = 0, r = (int)A.size() - 1;
        while(l < r){
            if(A[l] % 2 == 0){
                l++;
            } else if(A[r] % 2 == 1){
                r--;
            } else {
                swap(A[l++], A[r--]);
            }
        }
        return A;
    }
};
```

이 코드는 퀵정렬처럼 맨 왼쪽과 맨 오른쪽에서 좁혀가면서 배열을 검사하는 코드입니다.

최악의 경우는 제 코드와 같겠지만은, 배열의 반에서 딱 끝나는 경우가 있을 수 있으므로 좋은 코드입니다.  





