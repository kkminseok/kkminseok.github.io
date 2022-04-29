---
title: leetcode(리트코드)347-Top K Frequent Elements
author: 강민석
date: 2021-04-07 00:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 347 - Top K Frequent Elements 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/top-k-frequent-elements/>  

![](/assets/img/sample/leetcode/347/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/347/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 배열이 들어오고 k가 들어옵니다.
- k는 저장공간의 크기이고, 가장 많이 반복된 숫자들을 k만큼의 저장공간에 넣어 리턴합니다.
- map과 pq를 써야한다는 것은 알았지만 코드에 담아내지 못해 discuss를 참조했습니다. (그냥 못했다는 소리)

-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> um;
        for(size_t i = 0 ; i<nums.size();++i)
            um[nums[i]]++;
        vector<int> res;
        
        priority_queue<pair<int,int>> pq;
        
        for(auto it = um.begin(); it!=um.end(); ++it){
            pq.push(make_pair(it->second,it->first));
            if(pq.size() > (int)um.size() - k){
                res.push_back(pq.top().second);
                pq.pop();
            }
        }
        
        return res;
    }
};
```


-----

## 6. 결과 및 후기, 개선점


**c++ 90%**


![](/assets/img/sample/leetcode/347/result.JPG)  





