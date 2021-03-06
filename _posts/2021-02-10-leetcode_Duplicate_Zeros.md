---
title: leetcode(리트코드)1089-Duplicate Zeros
author: 강민석
date: 2021-02-10 12:08:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Duplicate Zeros 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/duplicate-zeros/>  

![](/assets/img/sample/leetcode/1089/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1089/input.JPG)

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  
eazy난이도의 문제입니다.  

-----  

## 4. 문제 해석

- 0이 나타나면 0을 하나 더 배열에 넣고 기존 배열값을 뒤로 한칸 미뤄 주어진 배열의 크기만큼을 리턴하는 문제입니다.  

-----  

## 5. code

```c++
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        vector<int> arr2;
        for(size_t i =0;i<arr.size();++i)
        {
            if(arr[i]==0)
                arr2.push_back(0);
            arr2.push_back(arr[i]);
            arr[i]=arr2[i];
        }
        
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/1089/result.JPG)  

굳이 개선할 점 없음.  
