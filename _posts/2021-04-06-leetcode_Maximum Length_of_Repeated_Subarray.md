---
title: leetcode(리트코드)718-Maximum Length of Repeated
author: 강민석
date: 2021-04-06 01:45:20 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 718 - Maximum Length of Repeated 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-length-of-repeated-subarray/>  

![](/assets/img/sample/leetcode/718/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/718/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
문제가 안풀려서 random으로 뽑아 푼 문제입니다.


-----  

## 4. 문제 해석

- LCS와 비슷하게 부분 숫자에서 공통된 연속된 숫자의 길이 중 큰 값을 리턴합니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int findLength(vector<int>& A, vector<int>& B) {
        int** DP = new int*[A.size()+1];
        for(int i = 0 ; i <A.size()+1;++i)
            DP[i] = new int[B.size()+1];
        int res = 0;
        for(int i = 0 ; i <A.size()+1;++i)
        {
            for(int j = 0 ; j <B.size()+1;++j)
            {
                if(i ==0 || j ==0) DP[i][j] =0;
                else
                {
                    if(A[i-1] == B[j-1]) DP[i][j] = DP[i-1][j-1] + 1;
                    else
                        DP[i][j] =0;
                    res = max(res,DP[i][j]);
                }
            }
        }
        
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!


**c++ 232ms 55%**


![](/assets/img/sample/leetcode/718/result.JPG)  








