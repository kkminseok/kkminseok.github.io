---
title: leetcode(리트코드)27-Remove Element
author: 강민석
date: 2021-02-10 12:04:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Remove Element 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-element/>  

![](/assets/img/sample/leetcode/27/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/27/input.JPG)

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  
eazy난이도의 문제입니다.  

-----  

## 4. 문제 해석

- val로 들어온 값을 새 배열을 만들지 않고 지워서 값을 리턴합니다. 다만, 지우고 남은 원소들을 앞으로 끄집어 내야합니다.  

-----  

## 5. code

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left =0;
        for(size_t i =0;i<nums.size();++i)
        {
            if(nums[i]==val)
                continue;
            else
            {
                nums[left++] =nums[i];
            }
        }
        return left;
        
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/27/result.JPG)  

굳이 개선할 점 없음.  
