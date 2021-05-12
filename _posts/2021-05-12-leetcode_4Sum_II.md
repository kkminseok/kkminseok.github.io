---
title: leetcode(리트코드)-454 4Sum II
author: 강민석
date: 2021-05-12 09:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 454 - 4Sum II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/4sum-ii/> 

![](/assets/img/sample/leetcode/454/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/454/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top Interview 문제입니다.


-----  

## 4. 문제 해석

- 벡터 4개가 들어옵니다.
- 4개의 벡터에서 요소를 하나씩 골라, 더한 값이 0이되는 경우의 수를 구해 리턴합니다.



-----  

## 5. code  

코드설명
- O(n^4)로 풀면 효율성 문제가 나므로, O(2*n^2)의 시간복잡도로 해결합니다.

**c++**

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int,int> um;
        for(auto a : nums1){
            for(auto b : nums2){
                um[a+b]++;
            }
        }
        int res = 0 ;
        for(auto c : nums3){
            for(auto d : nums4){
                //만약에 반대쌍인 경우가 있으면,
                auto it = um.find(0 - c - d);
                if(it!=um.end()){
                    //중복을 고려
                    res += it->second;
                }
            }
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 61% 4ms**

![](/assets/img/sample/leetcode/454/result.JPG)  



