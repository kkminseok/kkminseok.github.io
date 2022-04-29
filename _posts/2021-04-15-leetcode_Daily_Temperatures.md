---
title: leetcode(리트코드)739-Daily Temperatures
author: 강민석
date: 2021-04-15 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 739 - Daily Temperatures 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/decode-string/>https://leetcode.com/problems/daily-temperatures/  

![](/assets/img/sample/leetcode/739/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/739/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 온도 벡터가 들어옵니다. 각 인덱스를 기준으로 따뜻해지는 순간이 언제인지 벡터에 넣어 반환합니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        vector<int> res(T.size(),0);
        stack<pair<int,int>> st;
        
        for(size_t i = 0 ; i <T.size();++i){
            if(st.empty())st.push(make_pair(T[i],i));
            else{
                while(!st.empty() && st.top().first < T[i]){
                    int index = st.top().second;
                    res[index] = i - index;
                    st.pop();
                }
                st.push(make_pair(T[i],i));
            }
        }
        return res;
    }
};
```


-----

## 6. 결과 및 후기, 개선점


**c++ 95%**


![](/assets/img/sample/leetcode/739/result.JPG)  





