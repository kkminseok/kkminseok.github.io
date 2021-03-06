---
title: leetcode(리트코드)88-Merge Sorted Array
author: 강민석
date: 2021-02-10 12:06:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Merge Sorted Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/merge-sorted-array/>  

![](/assets/img/sample/leetcode/88/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/88/input.JPG)

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  
eazy난이도의 문제입니다.  

-----  

## 4. 문제 해석

- 말그대로 병합정렬 문제입니다. 보통 병합정렬과 다른 점은, 배열을 미리 주어준다는 점입니다. 보통 병합정렬은 새로운 배열을 생성해야하는데, 미리 준 배열에 병합정렬을 수행해야합니다.  


-----  

## 5. code

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        vector<int> num(nums1.size(),0);
        for(size_t i =0;i<nums1.size();++i)
        {
            num[i]=nums1[i];
        }
        int i=0,j=0,k=0;
        while(i<m && j<n)
        {
            if(num[i]<=nums2[j])
            {
                nums1[k++] = num[i++];
            }
            else
                nums1[k++] = nums2[j++];
        }
        int tmp = i > m ? j : i;
        while(i<m)
        {
            nums1[k++] = num[i++];
        }
        while(j<n)
            nums1[k++] = nums2[j++];

    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/88/result.JPG)  

이미 개선한 코드입니다.  
처음에는 함수를 하나 더 만들어줘서 배열들을 관리하였으나, 그럴 필요가 없다는 것을 알고 한 함수내에서 처리하도록 작성하였습니다.  

