---
title: leetcode(리트코드)1779-Find Nearest Point That Has the Same X or Y Coordinate
author: 강민석
date: 2021-03-09 04:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1779 - Find Nearest Point That Has the Same X or Y Coordinate 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-nearest-point-that-has-the-same-x-or-y-coordinate/>  

![](/assets/img/sample/leetcode/1779/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1779/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode 대회에서 나왔던 문제입니다.


-----  

## 4. 문제 해석

- 대회에서 풀 당시에는 해석이 안되어서.. 특히 'smallest index' 저 부분이 x좌표와 y좌표중 작은 값을 말하는 줄 알았는데 points의 인덱스를 말하는 것 이었습니다. 그래서 못품.. 하필 [2,4] 와 [4,4]에서 2라는 숫자때문에 더 헷갈렸습니다.
- 아무튼 의미를 깨닫고는 빠르게 풀 수 있었습니다.
- coordinate가 아닌것은 continue로 거르고 거리가 0인(동일한 점)을 찾으면 바로 리턴합니다.

-----  

## 5. code

**python**

```python
class Solution:
    def nearestValidPoint(self, x: int, y: int, points: List[List[int]]) -> int:
        ans = -1
        mind = 987654321
        for i in range(len(points)) : 
            if mind == 0 : 
                break
            x_d = x - points[i][0]
            y_d = y - points[i][1]
            
            if x_d != 0 and y_d !=0:
                continue
            distance = abs(x_d) + abs(y_d)
            if distance < mind:
                ans = i
                mind = distance
                
        return ans
```

**c++**
        
```c++
class Solution {
public:
    int nearestValidPoint(int x, int y, vector<vector<int>>& points) {
        int ans = -1;
        int mind = INT_MAX;
        for(size_t i =0;i<points.size() && mind != 0;++i)
        {
            int x_d = x - points[i][0];
            int y_d = y - points[i][1];
            
            if(x_d != 0 && y_d !=0)
            {
                continue;
            }
            int distance = abs(x_d) + abs(y_d);
            if(distance < mind)
            {
                ans = i;
                mind = distance;
            }
            
        }
        
        return ans;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1779/result.JPG)  


