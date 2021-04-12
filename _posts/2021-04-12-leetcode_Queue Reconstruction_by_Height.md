---
title: leetcode(리트코드)406-Queue Reconstruction by Height
author: 강민석
date: 2021-04-12 13:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 406 - Queue Reconstruction by Height 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/queue-reconstruction-by-height/>  

![](/assets/img/sample/leetcode/406/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/406/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- people이 들어옵니다. people[n][0]는 키이고 people[n][1]은 자신보다 같거나 큰 키를 가진 사람이 앞에 있어야하는 명 수 입니다.
- 위의 조건으로 재배열해야합니다.


-----  

## 5. code


**c++**

```c++
bool cmp(vector<int>& a,vector<int>& b){
    if(a[0] == b[0]) return a[1] < b[1];
    return a[0] > b[0];
}
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        vector<vector<int>> res;
        for(int i = 0 ; i<people.size();++i)
        {
            res.insert(res.begin() + people[i][1] ,people[i]);
        }
        return res;
    }

};

```


-----

## 6. 결과 및 후기, 개선점


**c++ 33%**


![](/assets/img/sample/leetcode/406/result.JPG)  





