---
title: leetcode(리트코드)128-Longest Consecutive Sequence
author: 강민석
date: 2021-04-02 03:45:20 +0800
categories: [leetcode,Hard]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 128 - Longest Consecutive Sequence 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-consecutive-sequence/>  

![](/assets/img/sample/leetcode/128/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/128/input.JPG)  


-----  

## 3. 분류 및 난이도

Hard 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다. 


-----  

## 4. 문제 해석

- 그래프로 분류되어 있는데 그래프로 안 풀었습니다. 그리디로 품.
- set으로 중복을 제거하고 순차적으로 접근하여 확인하였습니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if(nums.size()==0) return 0;
        
        set<int> st;
        int count =1;
        for(size_t i = 0;i<nums.size();++i){
            st.insert(nums[i]);
        }
        if(st.size()==1) return 1;
        int num = INT_MAX;
        int temp = 0;
        int answer = 1;
        for(auto it = st.begin(); it!=st.end();++it)
        {
            if(num==INT_MAX){num = *it; continue;}
            else{
                temp = num;
                num = *it;
                if(temp+1 != num) {
                    count=1;
                    continue;
                }
                ++count;
                answer = max(answer, count);
            }
            
        }
        
        return answer;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!


**c++ 4ms 98%**


![](/assets/img/sample/leetcode/128/result.JPG)  








