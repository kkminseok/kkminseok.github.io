---
title: leetcode(리트코드)3월03일 challenge268-Missing Number
author: 강민석
date: 2021-03-04 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 03일 - Missing Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/missing-number/>  

![](/assets/img/sample/leetcode/268/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/268/input.JPG)  

**Constraints:**

- n == nums.length
- 1 <= n <= 104
- 0 <= nums[i] <= n
- All the numbers of nums are unique.


-----  

## 3. 분류 및 난이도

eazy 난이도입니다.  
3월 03일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 0 ~ n의 만큼의 배열이 들어와야합니다. 한 숫자가 누락되었는데 그 숫자를 찾아야합니다.


-----  

## 5. code

**c++**  


```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        for(int i =0;i<nums.size();++i)
            if(nums[i] != i)
                return i;
        return nums[nums.size()-1]+1;
    }
};
```

**python**

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
            nums.sort()
            for i in range (0,len(nums)):
                if nums[i] != i :
                    return i
            return nums[len(nums)-1]+1;

```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/268/result.JPG)  

**24ms 100% 코드**

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int sum = 0;
        for(auto num : nums)
        {
            sum += num;
        }
        
        int numsLength = nums.size();
        int total = numsLength*(numsLength+1)/2;
        
        return total-sum;
    }
};
```

정렬이 없이 벡터의 모든 합을 더하고, 정상합에서 빼서 수를 찾습니다.

