---
title: leetcode(리트코드)34-Find First and Last Position of Element in Sorted Array
author: 강민석
date: 2021-02-21 13:10:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 34 - Find First and Last Position of Element in Sorted Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/>  

![](/assets/img/sample/leetcode/34/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/34/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- target으로 주어진 값을 찾아 반복되는 구간의 첫 부분과 끝 부분을 찾아 리턴합니다.

- 문제에서 시간복잡도 logN으로 해결하라고 하였으니, 이분탐색을 통해 문제를 해결하면 됩니다.

-----  

## 5. code

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int start = 0;
        int end=nums.size()-1;

        vector<int> result;
        while(start<=end)
        {
            int mid = (start+end)/2;
            cout<<start<<" "<<mid<<" "<<end<<'\n';
            if(target==nums[mid])
            {
                start = mid;
                end = mid;
                while(start-1 >=0 && nums[start-1] ==target)
                    start--;
                while(end+1 <nums.size() && nums[end+1] == target)
                    ++end;
                result.push_back(start);
                result.push_back(end);
                return result;
            }
            else if(target>nums[mid])
            {
                start = mid+1;
            }
            else
                end=mid-1;
        }
        result.push_back(-1);
        result.push_back(-1);
            
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**시간 4ms(97%)**


![](/assets/img/sample/leetcode/34/result.JPG)  




