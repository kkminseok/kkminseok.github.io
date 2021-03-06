---
title: leetcode(리트코드)2월15일 challenge1337-The K Weakest Rows in a Matrix
author: 강민석
date: 2021-02-15 17:10:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge1337 - The K Weakest 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/>  

![](/assets/img/sample/leetcode/1337/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1337/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월15일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- K번째로 약한 열까지 찾아 벡터에 넣어 리턴하는 것입니다. 1은 군인이고 0은 민간인으로 행의 군사의 수가 작을수록 약한 행입니다.


-----  

## 5. code

```c++
bool pred(pair<int,int> a,pair<int,int> b)
{
    if(a.second==b.second)
        return a.first<b.first;
    return a.second < b.second;
}

class Solution {
public:
    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        vector<int> result;
        vector<pair<int,int>> vec;
        for(size_t i = 0;i<mat.size();++i)
        {
            int sum = 0;
            for(size_t j =0;j<mat[i].size();++j)
            {
                if(mat[i][j]==1)
                    ++sum;
            }
            vec.push_back(make_pair(i,sum));
        }
        sort(vec.begin(),vec.end(),pred);
        
        for(int i=0;i<k;++i)
        {
            result.push_back(vec[i].first);
        }
        
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점
  
**시간(96%)**
![](/assets/img/sample/leetcode/1337/result.JPG) 

