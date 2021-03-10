---
title: leetcode(리트코드)75-Sort Colors
author: 강민석
date: 2021-03-10 02:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 75 - Sort Colors 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/sort-colors/>  

![](/assets/img/sample/leetcode/75/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/75/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 라이브러리를 사용하지 않고 정렬합니다.
- 기수정렬 비슷하게 풀었습니다.



-----  

## 5. code

**python**

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        ns = len(nums)
        countz = [0] * 3
        for i in range(ns):
            countz[nums[i]]+=1
        index = 0
        for i in range(ns):
            while index < 3 and countz[index]==0:
                index+=1
            nums[i] = index
            countz[index]-=1
```

----- 

**c++**

```c++   
class Solution {
public:
    void sortColors(vector<int>& nums) {
     int ns = nums.size();
     int countz[3] = {0,0,0};    
     for(size_t i =0;i<ns;++i)
     {
         countz[nums[i]]++;
     }
    int index =0;
    for(size_t i =0 ;i<ns;++i)
    {
        while(index<3 && countz[index]==0)
            ++index;
        nums[i] = index;
        countz[index]--;

    }
    }
};
```

-----

## 6. 결과 및 후기, 개선점

python 77%, c++ 100%

![](/assets/img/sample/leetcode/75/result.JPG)  


**python 100% 12ms code**

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        i=0
        j=len(nums)-1
        k=0
        while k<=j:
            if nums[k]==2:
                t=nums[k]
                nums[k]=nums[j]
                nums[j]=t
                j-=1
            elif k!=i and nums[k]==0:
                t=nums[k]
                nums[k]=nums[i]
                nums[i]=t
                i+=1
            else:
                k+=1
                
```
i는 0을 관리하고 j 는 끝에서 2와 바꿔줄 인덱스값을 관리합니다.
인덱스의 값이 1인 경우는 elif문에서 swap을 하면서 바꿔버립니다.