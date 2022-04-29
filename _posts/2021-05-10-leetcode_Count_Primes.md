---
title: leetcode(리트코드)5월10일 challenge204-Count Primes
date: 2021-05-10 01:01:56 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 10일 - Count Primes 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-primes/>  

![](/assets/img/sample/leetcode/204/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/204/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 10일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- n으로 들어온 자연수값의 약수 개수를 구해서 리턴합니다.





-----  

## 5. code

에라토스테네스의 체를 이용하면 쉽게 풀 수 있습니다.


**c++**

```c++
class Solution {
public:
    int countPrimes(int n) {
        //에라토스테네스의 체
        if(n==0 || n==1)
            return 0;
        int* dp = new int[n+1];
        int res = 0;
        dp[0] = 0;
        dp[1] =0;
        
        for(int i = 2 ; i< n+1;++i)
            dp[i] = i;
        
        for(int i = 2; i<sqrt(n+1); ++i){
            if(dp[i] == 0) continue;
            for(int j = i*2 ; j<n+1 ; j +=i)
                dp[j]=0;
        }
        for(int i = 2;i<n;++i)
                if(dp[i]!=0) ++res;
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/204/result.JPG)  




