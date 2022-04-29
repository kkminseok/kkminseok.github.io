---
title: leetcode(리트코드)2월23일 challenge240-Search a 2D Matrix II 
author: 강민석
date: 2021-02-23 00:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February 240 - Search a 2D Matrix II 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/search-a-2d-matrix-ii/>  

![](/assets/img/sample/leetcode/240/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/240/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월23일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 오름차순이 되어 있는 2차원 배열을 돌면서 target값을 효율성있게 찾아야합니다. 
- STL 에서 find 함수를 지원하지만 find 함수는 순차접근으로 계산하기에 효율성이 좋은 코드는 아닙니다.
- BinarySearch를 이용해서 위의 코드를 개선하였습니다.

-----  

## 5. code

```c++

class Solution {
public:

    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        for(size_t i =0;i<matrix.size();++i)
        {
            auto finditem = binary_search(matrix[i].begin(),matrix[i].end(),target);
            if(finditem)
                return true;
        }
        return false;
        
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/240/result.JPG)  


**시간이 빠른 코드 12ms(100%)**

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& a, int target) {
        ios::sync_with_stdio(0);
        //h는 배열의 행 w는 열
        int h = 0, w = a[0].size(), x;
        while(h < a.size() && w && (x = a[h][w-1]) != target) {
            if (x > target) w--;
            else h++;
        }
        //while문은 기존 코드에 있으나 지워도 상관없어서 지워버림.
        //while (a.size()) a.pop_back();
        return x == target;
    }
};
```
일단 열의 끝 부터 검사하면서 그 다음 행을 검사합니다.  