---
title: leetcode(리트코드)300-Longest Increasing Subsequence
author: 강민석
date: 2021-03-23 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 300 - Longest Increasing 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-increasing-subsequence/>  

![](/assets/img/sample/leetcode/300/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/300/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- DP문제입니다.
- 부분수열중에서 가장 긴 증가수열의 길이를 찾아 리턴합니다.



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int* DP =new int[nums.size()];
        for(int i =0 ;i<nums.size();++i)
            DP[i] = 1;
        int result = 1;
        for(int i = 1; i<nums.size();++i)
        {
            for(int j =0;j<i;++j)
            {
                if(nums[i]> nums[j])
                    DP[i] = max(DP[i],DP[j]+1);
                result = max(result,DP[i]);
            }
        }
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 56% python ??%** 
![](/assets/img/sample/leetcode/300/result.JPG)  


**O(nlogn)으로 푼 코드 0ms 100%**

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        vector<int> lis(n, 0);
        for(int i=0; i<n; i++)
        {
            int lo=0, hi=ans;
            while(lo<hi)
            {
                int mid = lo + (hi-lo)/2;
                if(lis[mid]<nums[i]) lo = mid+1;
                else hi = mid;
            }
            lis[lo] = nums[i];
            if(lo==ans) ans++;
        }
        return ans;
    }
};
```

이분 검색으로 값을 찾아서 갱신하는 코드입니다.




