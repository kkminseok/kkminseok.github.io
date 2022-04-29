---
title: leetcode(리트코드)1299-Replace Elements with Greatest Element on Right Side    
author: 강민석
date: 2021-02-12 12:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Replace Elements with Greatest Element on Right Side 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/replace-elements-with-greatest-element-on-right-side/>  

![](/assets/img/sample/leetcode/1299/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1299/input.JPG)

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 첫번째 요소들을 돌면서 그 오른쪽에 있는 요소들 중 가장 큰값을 배열에 넣어 리턴합니다.  

-----  

## 5. code

```c++
class Solution {
public:
    vector<int> replaceElements(vector<int>& arr) {
     for(size_t i =arr.size()-1;i>0;--i)
     {
         if(arr[i] > arr[i-1])
         {
             arr[i-1]=arr[i];
         }
     }
        arr.erase(arr.begin());
        arr.push_back(-1);
        return arr;     
    }

};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/941/result.JPG)  

효율성이 좋은 다른 코드와 로직이 비슷하므로 개선점은 없습니다.  



