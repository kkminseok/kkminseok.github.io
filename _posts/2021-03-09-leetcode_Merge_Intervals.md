---
title: leetcode(리트코드)56-Merge Intervals
author: 강민석
date: 2021-03-09 02:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 56 - Merge Intervals 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/merge-intervals/>  

![](/assets/img/sample/leetcode/56/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/56/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 구간의 시작과 끝이 주어집니다. 겹치는 구간은 합쳐버려서 구간을 다시 리턴합니다.


-----  

## 5. code

**python**

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        result =[]
        intervals.sort()
        index = 0
        while index < len(intervals):
            temp = []
            start = intervals[index][0]
            end = intervals[index][1]
            j = index+1
            while j < len(intervals) and end >= intervals[j][0] : 
                end = max(intervals[j][1],end)
                index = j
                j += 1
            temp.append(start)
            temp.append(end)
            result.append(temp)
            index+=1
        return result
        
```

-----  

**c++**

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        sort(intervals.begin(),intervals.end());
        for(int i = 0; i<intervals.size(); ++i)
        {
            vector<int> temp;
            int start = intervals[i][0];
            int end = intervals[i][1];
            int j = i+1;
            while(j<intervals.size() && end >= intervals[j][0])
            {
                end = max(intervals[j][1],end);
                i=j;
                ++j;
            }
            temp.push_back(start);
            temp.push_back(end);
            result.push_back(temp);
        }
        
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

94%

![](/assets/img/sample/leetcode/56/result.JPG)  


