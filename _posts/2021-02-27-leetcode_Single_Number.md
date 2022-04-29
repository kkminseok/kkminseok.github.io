---
title: leetcode(리트코드)136-Single Number
author: 강민석
date: 2021-02-27 16:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 136 - Single Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/single-number/>  

![](/assets/img/sample/leetcode/136/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/136/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 한 요소 빼고 다른 요소들은 2번씩 반복됩니다. 2번씩 반복되지 않은 요소를 찾아 리턴합니다.


-----  

## 5. code

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int result = 0;
        for(size_t i = 0; i<nums.size();i+=2)
        {
            if(i ==nums.size()-1|| nums[i] != nums[i+1])
            {
                result = nums[i];
                break;
            }
        }
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/136/result.JPG)  


**4ms 100% 코드**

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int a =0;
        for (int n: nums) a^=n;
        return a;
    }
};
```

XOR 비트연산을 통해 값을 도출하는 코드입니다. 어차피 2번 반복되면 비트는 0으로 바뀔 것이므로 가능한 것입니다.
배웠습니다.  


