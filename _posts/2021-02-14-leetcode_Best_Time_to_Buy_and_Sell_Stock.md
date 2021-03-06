---
title: leetcode(리트코드)121-Best Time to Buy and Sell Stock
author: 강민석
date: 2021-02-14 15:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 121 - Best Time to Buy and Sell Stock 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/best-time-to-buy-and-sell-stock/>  

![](/assets/img/sample/leetcode/121/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/121/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked의 세 번째 문제입니다.  


-----  

## 4. 문제 해석

- 배열을 돌면서 물건을 살 때와 물건을 팔 때를 정하여 최대 이익을 구하고 그 값을 리턴합니다.
- 지나온 날로 돌아가 물건을 팔 수 없습니다.  


- 배열의 크기가 10^5이므로 for문을 두 번 쓰면서 순차탐색으로는 문제를 풀 수 없습니다.
- 지나오면서 최소값을 **저장**하면 되고 최소값을 뺐을 때의 결과 값을 **저장**하여 비교하면 풀 수 있습니다.





-----  

## 5. code

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        int minprice = INT_MAX;
        for(size_t i =0;i<prices.size();++i)
        {
            minprice= min(minprice,prices[i]);
            result = max(result, prices[i]-minprice);
        }
        
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(40%)**  

![](/assets/img/sample/leetcode/121/result.JPG)



**빠른 시간 코드(0ms 99%)**
```c++
class Solution {
public:
    int maxProfit(vector<int>& p) {
        if(p.size()<=1)
            return 0;
        int mn=p[0],prof=0,n=p.size();
        for(int i=1;i<n;i++)
        {
            mn=min(mn,p[i]);
            prof=max(prof,p[i]-mn);
        }
        
        return prof;
    }
};
```

똑같은데..? 뭐지