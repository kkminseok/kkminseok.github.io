---
title: leetcode(리트코드)283-Move Zeroes    
author: 강민석
date: 2021-02-12 12:30:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Move Zeroes 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/move-zeroes/>  

![](/assets/img/sample/leetcode/283/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/283/input.JPG)

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 배열을 돌면서 0인것은 맨뒤로 아닌것은 앞으로 보내는 문제입니다.  



-----  

## 5. code

```c++
class Solution {
public:    
    void moveZeroes(vector<int>& nums) {
        for(size_t i=0;i<nums.size();++i)
        {
            if(nums[i]==0)
            {
                for(int j=i+1;j<nums.size();++j)
                {
                    if(nums[j]!=0)
                    {
                        int temp = nums[i];
                        nums[i] = nums[j];
                        nums[j] =temp;
                        break;
                    }
                }
            }
        }
        
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/283/result.JPG)  

시간이 8%로 하위에서 벗어나질 못했습니다.  

작성하면서도 마음에 걸렸던 2중 for문이 시간을 많이 잡아먹은 것 같다고 생각하고 다른 사람들 코드를 보니 전부 1중 for문으로 해결했습니다.  

**시간 효율성이 좋은 코드**

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        for(int i = 0, j = 0; i<nums.size(); ++i) {
            if(nums[i])
                swap(nums[i], nums[j++]);
        }
    }
};
```
0이면 j를 냅두고 0이 아닌경우 i를 증가시켜 0에 위치된 j와 값을 바꾸는 코드입니다.  





