---
title: leetcode(리트코드)3월30일 challenge354-Russian Doll Envelopes
author: 강민석
date: 2021-03-30 15:00:56 +0800
categories: [leetcode,Hard]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 30일 - Russian Doll Envelopes 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/russian-doll-envelopes/>  

![](/assets/img/sample/leetcode/354/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/354/input.JPG)  


-----  

## 3. 분류 및 난이도

Hard 난이도입니다.  
3월 30일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 벡터안에 봉투의 width와 height가 주어집니다. 작은 봉투에서 넓은 봉투로 담을 때 가장 큰 담는 횟수를 구하세요.
- DP로 풀었지만, O(n^2)이기에효율이 좋지는 않습니다.



-----  

## 5. code

**c++**

```c++
bool pred(vector<int>& a,vector<int>& b){
    if(a[0] ==b[0])
        return a[1]<b[1];
    return a[0]<b[0];
}
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        if(envelopes.size()==0)
            return 0;
        int* DP = new int[envelopes.size()];
        int h = envelopes.size();
        int w = envelopes[0].size();
        int result =0;
        sort(envelopes.begin(),envelopes.end(),pred);
        for(int i = 0;i<h;++i){
            DP[i] = 1;
            for(int j = 0;j<i;++j){
                if(envelopes[i][0] > envelopes[j][0] && envelopes[i][1] > envelopes[j][1]){
                    DP[i] = max(DP[i], 1+DP[j]);
                }
            }
            result= max(result,DP[i]);
        }
        return result;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**c++ 46%**
![](/assets/img/sample/leetcode/354/result.JPG)  
