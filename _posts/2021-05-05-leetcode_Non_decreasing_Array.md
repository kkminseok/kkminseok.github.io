---
title: leetcode(리트코드)5월4일 challenge665-Non decreasing Array
date: 2021-05-05 10:17:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 4일 - Non decreasing Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/non-decreasing-array/>  

![](/assets/img/sample/leetcode/665/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/665/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 4일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- nums라는 벡터가 들어옵니다.
- 하나의 값만 수정해서 오름차순으로 만들 수 있으면 true를 만들 수 없으면 false를 리턴합니다.





-----  

## 5. code


**c++**

```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int res = 0 ;
        for(int i = 1 ; i <nums.size() && res<=1 ; ++i){
            if(nums[i]<nums[i-1]){
                res++;
                if(i-2<0 || nums[i-2] <= nums[i]) nums[i-1]= nums[i];
                else
                    nums[i] = nums[i-1];
            }
        }
        return res<=1;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/665/result.JPG)  




