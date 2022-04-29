---
title: leetcode(리트코드)55-Jump Game
author: 강민석
date: 2021-02-27 13:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 55 - Jump Game 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/jump-game/>  

![](/assets/img/sample/leetcode/55/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/55/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 벡터로 주어진 요소만큼 점프할 수 있습니다. 점프를 해서 맨 끝요소로 도달할 수 있는지 여부를 리턴합니다.


-----  

## 5. code

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        bool* v = new bool[nums.size()];
        memset(v,false,sizeof(bool) * nums.size());
        v[0]=true;
        for(size_t i =0;i<nums.size()-1;++i)
        {
            if(v[i]==true)
            {
                for(int j =0;j<=nums[i]; ++j)
                {
                    if(i+j>nums.size()-1)
                    {
                        break;
                    }
                    if(v[i+j]==true)
                        continue;
                    else
                        v[i+j]=true;
                }
            }
        }
        return v[nums.size()-1];
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/55/result.JPG)  


**0ms 100% 코드**

```c++
class Solution {
public:
    
    
    bool canJump(vector<int>& nums) {
        
        int s=nums.size();
        //벡터의 크기가 1개일 때
        if(s==1){
            return true;
        }
        
        int target=s-1;
        
        for(int i=s-2; i>=0; i--){
            
            if(i+nums[i]>=target){
                target=i;
            }
            
        }
        
        return target==0;  
    }
    
       
};
```

백트래킹 방법입니다. 만약 더 많이 점프할 수 있으면 해당 인덱스로 옮겨버립니다.


