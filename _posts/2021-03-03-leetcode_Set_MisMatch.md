---
title: leetcode(리트코드)3월02일 challenge645-Set Mismatch
author: 강민석
date: 2021-03-03 00:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 02일 - Set Mismatch 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/set-mismatch/>  

![](/assets/img/sample/leetcode/645/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/645/input.JPG)  


-----  

## 3. 분류 및 난이도

eazy 난이도입니다.  
3월 02일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 0 ~ n의 만큼의 배열이 들어와야합니다. 한 숫자가 누락되었는데 그 숫자를 찾아야합니다.


-----  

## 5. code

**c++**  


**c++**

```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        
        bool* countnum = new bool[nums.size()+1];
        memset(countnum,false,sizeof(bool) * (nums.size()+1));
        vector<int> result;
        for(size_t i=0;i<nums.size();++i)
        {
            if(countnum[nums[i]])
                result.push_back(nums[i]);
            countnum[nums[i]]=true;
        }
        bool check =false;
        for(size_t i=1;i<nums.size()+1;++i)
        {
            if(!countnum[i])
            {
                result.push_back(i);
                break;
            }
        }
        
        return result;
        
    }
};
```



**python**


```python
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        countnum = [0] * (len(nums)+1)
        result = []
        for i in range(len(nums)):
            if(countnum[nums[i]]):
                result.append(nums[i])
            countnum[nums[i]]=1
        check = 0
        for i in range(1,len(nums)+1):
            if countnum[i] == 0:
                result.append(i)
                break
        return result
        
```

-----

## 6. 결과 및 후기, 개선점

95%

![](/assets/img/sample/leetcode/645/result.JPG)  

