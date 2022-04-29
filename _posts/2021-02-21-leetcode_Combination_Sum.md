---
title: leetcode(리트코드)39-Combination Sum
author: 강민석
date: 2021-02-21 14:10:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 39 - Combination Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/combination-sum/>  

![](/assets/img/sample/leetcode/39/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/39/input.JPG)  

**Constraints:**

- 1 <= candidates.length <= 30
- 1 <= candidates[i] <= 200
- All elements of candidates are distinct.
- 1 <= target <= 500

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 후보자로 주어진 벡터에서 target을 만들 수 있는 경우의 수를 벡터에 넣어 리턴합니다.

-----  

## 5. code

```c++
class Solution {
public:
    vector<vector<int>> result;
    void practice(vector<int>& candidates, int target,vector<int> temp,int num)
    {
        if(target==0)
        {
               result.push_back(temp);
        }
        else if(target<0)
        {
            return;
        }
        else
        {
            for( ;num<candidates.size();++num)
            {
                temp.push_back(candidates[num]);
                practice(candidates,target-candidates[num],temp,num);
                temp.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> temp;
        practice(candidates,target,temp,0);
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**시간 40ms(30%)**


![](/assets/img/sample/leetcode/39/result.JPG)  

이해가 안되는 점은 4ms 97% 코드와 똑같은데 40ms 나온 것.. 트래픽이 몰렸나..?

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> temp;
        vector<vector<int>> result;
        solve(candidates, 0, target, temp, result);
        return result;
    }
    void solve(vector<int>& candidates, int start, int target, vector<int> &temp, vector<vector<int>>&result) {
        if(target < 0) {
            return;
        }
        else if(target == 0) {
            result.push_back(temp);
        }
        for(int i=start ; i< candidates.size() ; i++) {
            temp.push_back(candidates[i]);
            solve(candidates, i, target-candidates[i], temp, result);
            temp.pop_back();
        }
    }
};
```




