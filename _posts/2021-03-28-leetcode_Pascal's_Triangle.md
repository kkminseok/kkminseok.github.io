---
title: leetcode(리트코드)118-Pascal's Triangle
author: 강민석
date: 2021-03-28 02:12:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 118 - Pascal's Triangle 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/pascals-triangle/>  

![](/assets/img/sample/leetcode/118/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/118/input.JPG)  

```console
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- 들어온 numRows값의 row를 가진 파스칼 삼각형을 구현합니다.

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result;
        for(int i =0;i<numRows;++i)
        {
            vector<int> jvec;
            jvec.push_back(1);
            for(int j =1;j<i;++j)
            {
                jvec.push_back(result[i-1][j-1] + result[i-1][j]);
            }
            if(i!=0)
                jvec.push_back(1);
            result.push_back(jvec);
        }
        
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python ??%** 
![](/assets/img/sample/leetcode/118/result.JPG)  






