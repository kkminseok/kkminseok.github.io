---
title: leetcode(리트코드)26-Remove Duplicates from Sorted Array
author: 강민석
date: 2021-02-10 12:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Remove Duplicates from Sorted Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-duplicates-from-sorted-array/>  

![](/assets/img/sample/leetcode/26/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/26/input.JPG)

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  
eazy난이도의 문제입니다.  

-----  

## 4. 문제 해석

- 중복되는 수를 제거하여 주어진 배열을 바꾸는 문제입니다.  
문제에서 새 배열을 만들지 말고 O(1)로 푸는 것을 권장하였습니다.  




-----  

## 5. code

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int left =0;
        for(size_t i=0;i<nums.size();++i)
        {
            nums[left++] = nums[i];
            while(i+1< nums.size() && nums[i] == nums[i+1])
            {
                ++i;
            }
        }
        return left;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/26/result.JPG)  

굳이 개선할 점 없음.  
