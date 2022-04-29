---
title: leetcode(리트코드)169-Major Element
author: 강민석
date: 2021-03-16 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 169 - Major Element 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/majority-element/>  

![](/assets/img/sample/leetcode/169/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/169/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- Major한 원소를 찾아야합니다. 여기서 Major한 원소는 배열의 절반 이상 등장하는 요소로, 무조건 있다고 가정합니다.
- 절반 이상을 차지하고 있다고 보장하므로 배열의 각 요소에 대한갯수를 더하고 빼면 결국 남은 요소는 Major한 요소일 것입니다.

-----  

## 5. code

**c++**


```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int major = nums[0];
        int count = 1;
        for(size_t i = 1;i<nums.size();++i)
        {
            if(count==0)
            {
                major = nums[i];
                ++count;
            }
            else if(nums[i]==major)
                ++count;
            else
                --count;
        }
        return major;
    }
};
```

-----  

**python**

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 1
        major = nums[0]
        for i in range(1,len(nums)):
            if count == 0 :
                major = nums[i]
                count = 1
            elif nums[i] == major : 
                count += 1
            else : 
                count -= 1
        return major
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 70% python 88%**  
![](/assets/img/sample/leetcode/169/result.JPG)  


