---
title: leetcode(리트코드)53-Maximum Subarray
author: 강민석
date: 2021-02-21 15:10:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 53 - Maximum Subarray 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-subarray/>  

![](/assets/img/sample/leetcode/53/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/53/input.JPG)  

**Constraints:**

- 1 <= nums.length <= 3 * 104
- -105 <= nums[i] <= 105


-----  

## 3. 분류 및 난이도

Hard 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 주어진 배열내에서 연속된 수를 더해 가장큰 값을 찾습니다.
- DP로 풀었습니다.

-----  

## 5. code

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.size()==1)
            return nums[0];
        int* DP = new int[nums.size()+1];
        DP[0] = nums[0];
        int result = DP[0];
        for(size_t i =1;i<nums.size();++i)
        {
            DP[i] = max(DP[i-1] + nums[i], nums[i]);
            result = max(result,DP[i]);
        }
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/53/result.JPG)  


0ms 코드와 같습니다.  

