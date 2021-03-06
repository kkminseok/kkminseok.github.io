---
title: leetcode(리트코드)1-Two Sum
author: 강민석
date: 2021-02-16 05:10:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1 - Two Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/two-sum/>  

![](/assets/img/sample/leetcode/1/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 배열의 요소를 더해서 target을 만들 수 있으면 해당 인덱스를 벡터에 넣고 반환합니다.  
- 답은 1가지로 주어지므로 찾으면 바로 리턴합니다.


-----  

## 5. code

**c++**

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result;
        for(size_t i=0;i<nums.size();++i)
        {
            for(size_t j = i+1; j<nums.size();++j)
            {
                if(nums[i] + nums[j]==target)
                {
                    result.push_back(i);
                    result.push_back(j);
                    return result;
                }
            }
        }
        return result;
    }
};
```

**python**

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        result=[]
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                if nums[i] + nums[j] == target:
                    result.append(i)
                    result.append(j)
                    return result
```


-----

## 6. 결과 및 후기, 개선점

**시간(100%)**  

![](/assets/img/sample/leetcode/1/result.JPG)

