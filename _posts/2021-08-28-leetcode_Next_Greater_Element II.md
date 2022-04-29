---
title: leetcode(리트코드)-503 Next Greater Element II(PYTHON)
author: 강민석
date: 2021-08-28 01:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 503 - Next Greater Element II  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/next-greater-element-ii/> 

![](/assets/img/sample/leetcode/503/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/503/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 배열을 볼 때 인덱스를 기준으로 오른쪽으로 탐색할 때 해당 인덱스보다 큰 값을 결과 리스트에 담아 리턴합니다. 만약 제일 큰 값이면 -1를 넣고, 기존 리스트의 맨 끝까지 탐색했는데 없다면 다시 맨앞부터 탐색하여 찾습니다.





-----  

## 5. code  

문제해석

- stack을 사용해야합니다. 만약 배열이 [1,5,2,3,6,4]라고 할 경우 5,2,3,6에서 5와 3은 6을 넣어야하는데, 그 중간에 있는 2를 6을 넣어줘야하는 지 여부를 stack으로 따져야하기 때문입니다.    


**python**

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        stack = deque()
        sz = len(nums)
        res = [-1] * sz
        # 2바퀴를 돌기 위해서
        for i in range(sz * 2): 
            while stack and nums[stack[-1]] < nums[i%sz] : 
                res[stack.pop()] = nums[i % sz]
            stack.append(i%sz)
        return res
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/503/result.JPG)  



필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


