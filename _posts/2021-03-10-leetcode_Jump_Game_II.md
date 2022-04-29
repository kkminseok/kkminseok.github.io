---
title: leetcode(리트코드)45-Jump Game II
author: 강민석
date: 2021-03-10 00:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 45 - Jump Game II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/jump-game-ii/>  

![](/assets/img/sample/leetcode/45/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/45/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 배열에 들어있는 숫자만큼 최대로 점프할 수 있습니다. 점프를 많이하지 않고 끝에 도달하였을 때 점프한 횟수를 리턴하세요.



-----  

## 5. code

**python**
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        counting = [0] * len(nums)
        for i in range(len(nums)):
            for j in range(1,nums[i]+1):
                if i + j >= len(nums):
                    continue
                if counting[i+j] == 0 :
                    counting[i+j] = counting[i]+1
                    
        return counting[len(nums)-1]
```    

-----  

**c++**
        
```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        vector<int> v(nums.size(),0);
        v[0] = 0;
        for(size_t i =0;i<nums.size()-1;++i)
        {
           for(int j = 1;j<=nums[i] ; ++j)
           {
               if(i+j >=nums.size())
                   continue;
               if(v[i+j]==0)
                   v[i+j] = v[i]+1;
           }
        }
        return v[v.size()-1];
        
    }
};
```


-----

## 6. 결과 및 후기, 개선점

python, c++ 100%

![](/assets/img/sample/leetcode/45/result.JPG)  

