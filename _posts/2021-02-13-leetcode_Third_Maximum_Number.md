---
title: leetcode(리트코드)414-Third Maximum Number
author: 강민석
date: 2021-02-13 12:40:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Maximum Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/third-maximum-number/>  

![](/assets/img/sample/leetcode/414/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/414/input.JPG)

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 3번째로 큰값을 찾는 문제입니다. 3번째로 큰 값이 없으면 가장 큰 값을 찾아 return 합니다.  


-----  

## 5. code

```c++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
     sort(nums.begin(),nums.end(),greater<int>());
     int maxnum = nums[0];
     int count =0;
     for(size_t i=0;i<nums.size();++i)
     {
         if(nums[i]<maxnum)
         {
             ++count;
             maxnum = nums[i];
             if(count>1)
                 return maxnum;
         }
     }
     return nums[0];
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/414/result.JPG)  

**0ms 코드**

```c++
class Solution {
public:
    int thirdMax(vector<int>& a) {
        long one ,two,three;
        // 세 변수 모드 Long형 최소값을 넣습니다.
        one = two = three = LONG_MIN;
        // vector a를 돌면서
        for(auto i : a){
            
            if(i == one || i == two || i==three) continue;
            // 배열의 인덱스가 처음으로 one보다 큰 값이 나오면 one, two, three 변수들의 값을 옮겨줍니다. 
            if(i>one){
                three = two;
                two = one;
                one = i;
            }
            //마찬가지로 인덱스로 들어온 값이 two보다 큰 값이 들어오면 값을 옮겨줍니다. else if문이라 두 번째로 걸리게 됩니다.
            else if(i>two){
                three = two;
                two = i;
            }
            //마찬가지
            else if(i>three){
                three = i;
            }
        }
        return three == LONG_MIN ? one:three;
    }
};
```

이게 왜 0ms인지는 잘 모르겠습니다.  



