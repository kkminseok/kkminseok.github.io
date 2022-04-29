---
title: leetcode(리트코드)-1700 Number of Students Unable to Eat Lunch(PYTHON)
author: 강민석
date: 2021-08-09 00:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1700 - Number of Students Unable to Eat Lunch  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/number-of-students-unable-to-eat-lunch/> 

![](/assets/img/sample/leetcode/1700/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1700/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- students는 queue로 이루어져있고, sandwiches는 stack으로 이루어져 있다고 가정합니다.
- students의 맨 앞에 있는 숫자와 sandwiches의 맨 앞에 있는 숫자와 같으면 두 요소를 삭제시켜주고, 다르면 students의 맨 앞 요소를 맨 뒤로 보냅니다.
- 위의 과정을 반복할 때 남아있는 students의 수를 리턴하세요.


-----  

## 5. code  

코드설명




**python**

```python
class Solution:
    def countStudents(self, students: List[int], sandwiches: List[int]) -> int:
        #brute
        students = deque(students)
        sandwiches = deque(sandwiches)
        check = True
        idx = 0
        while check : 
            check = False
            if len(sandwiches) != 0 :
                idx = sandwiches[0]
            for i in range(len(students)) : 
                temp = students.popleft()
                if idx == temp : 
                    sandwiches.popleft()
                    check = True
                    break
                else : 
                    students.append(temp)
        return len(sandwiches)            
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1700/result.JPG)  


필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


