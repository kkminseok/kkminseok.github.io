---
title: leetcode(리트코드)48-Rotate Image
author: 강민석
date: 2021-02-21 15:30:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 48 - Rotate Image 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/rotate-image/>  

![](/assets/img/sample/leetcode/48/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/48/input.JPG)  


**Constraints:**

- matrix.length == n
- matrix[i].length == n
- 1 <= n <= 20
- -1000 <= matrix[i][j] <= 1000

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 새로운 2차원 벡터를 생성하지 않고 주어진 벡터를 90도 오른쪽으로 돌려야합니다.

-----  

## 5. code

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int size = matrix.size();

       for(size_t i =0;i<size;++i)
       {
           vector<int> temp;
           for(int j = size-1;j>-1;--j)
           {
               temp.push_back(matrix[j][0]);
               matrix[j].erase(matrix[j].begin());
           }
           matrix.push_back(temp);    
       }
        for(int i =0 ;i<size;++i)
            matrix.erase(matrix.begin());
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/48/result.JPG)  







