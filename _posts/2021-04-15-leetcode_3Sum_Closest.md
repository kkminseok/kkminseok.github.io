---
title: leetcode(리트코드)16-3Sum Closest
author: 강민석
date: 2021-04-15 12:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 16 - 3Sum Closest 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/3sum-closest/>  

![](/assets/img/sample/leetcode/16/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/16/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 들어온 벡터에서 3개의 요소를 더한 값이 최대한 target과 비슷한 것을 찾아 리턴합니다.


-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        //tiem out
        int res = 0;
        int min = INT_MAX;
        for(int i = 0 ; i<nums.size()-2;++i){
            int sum = nums[i];
            for(int j = i+1; j< nums.size()-1; ++j){
                sum += nums[j];
                for(int k = j+1; k< nums.size();++k){
                    sum+=nums[k];
                    int temp = target-sum;
                    if(abs(min) > abs(temp))
                    {
                        min = temp;
                        res = sum;
                    }
                        
                    sum-=nums[k];
            }
                sum-=nums[j];
        }
    }
        return res;
    }
};
```


-----

## 6. 결과 및 후기, 개선점


**c++ 5%**


![](/assets/img/sample/leetcode/16/result.JPG)  





**개선한 0ms code 100%**

```c++
class Solution {
 public:
  int threeSumClosest(vector<int>& v, int target) {
    sort(v.begin(), v.end());
    int res = v[0] + v[1] + v[2];

    int n = v.size();
    for (int i = 0; i < n - 2; i++) {
        //k는 끝에서부터 검사
      for (int j = i + 1, k = n - 1; j < k;) {
        int curr = v[i] + v[j] + v[k];
        if (abs(curr - target) < abs(res - target)) res = curr;

        if (curr - target == 0)
          return curr;
        else if (curr - target > 0)
          k--;
        else
          j++;
      }
    }

    return res;
  }
};
```


저의 코드인 for문을 3개쓴 O(n^3)에 비해 for문을 2개 쓰고 그 하나도 투 포인트탐색을 하므로 O(n^2)의 시간복잡도를 갖습니다.
