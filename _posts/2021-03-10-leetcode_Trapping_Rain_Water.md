---
title: leetcode(리트코드)42-Trapping Rain Water
author: 강민석
date: 2021-03-10 04:00:00 +0800
categories: [leetcode,Hard]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 42 - Trapping Rain Water 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/trapping-rain-water/>  

![](/assets/img/sample/leetcode/42/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/42/input.JPG)  

-----  

## 3. 분류 및 난이도

Hard 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- black으로 되어진 막대기들이 있습니다. 물이채워질 때 넓이를 구하세요. 한 칸의 넓이는 1입니다.

- 1시간 정도 풀다가 Discuss를 봤습니다.
- left와 right를 관리하여 푸는 방식인데, 코드 자체는 어렵지 않으나 이해하면 놀라운?.. 코드입니다..

-----  

## 5. code

**python**

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        left = 0
        right = len(height)-1
        maxleft = 0
        maxright = 0
        result = 0
        
        while left <= right:
            if height[left] <= height[right] :
                if maxleft <= height[left] : 
                    maxleft = height[left]
                else:
                    result += maxleft - height[left]
                left+=1
            else :
                if maxright <= height[right] : 
                    maxright = height[right]
                else:
                    result += maxright - height[right]
                right-=1
        return result
```

-----

**c++**

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int left = 0;
        int right = height.size()-1;
        int result = 0;
        int maxleft = 0;
        int maxright = 0;
        
        while(left<=right)
        {
            if(height[left]<=height[right])
            {
                if(height[left]>=maxleft) maxleft = height[left];
                else
                    result += maxleft - height[left];
                ++left;
            }
            else
            {
                if(height[right]>=maxright) maxright = height[right];
                else
                    result += maxright - height[right];
                --right;
            }
        }
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

Discuss를 봤으므로 올리지 않습니다.