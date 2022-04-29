---
title: leetcode(리트코드)122-Best Time to Buy and Sell Stock II
author: 강민석
date: 2021-03-30 01:01:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 122 - Best Time to Buy and Sell 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/>  

![](/assets/img/sample/leetcode/122/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/122/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
Top 100 Interview 문제입니다.  


-----  

## 4. 문제 해석

- DP문제이고, 비슷한 문제가 너무 많습니다. 다른 포스팅에도 해당 문제가 있으니 참고해주시길 바랍니다.



-----  

## 5. code


**c++**

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int* buy = new int[prices.size()];
        int* sell = new int[prices.size()];
        buy[0] = -prices[0];
        sell[0] = 0;
        int result = 0;
        for(int i =1;i<prices.size();++i){
            buy[i] = max(buy[i-1],sell[i-1] - prices[i]);
            sell[i] = max(sell[i-1],buy[i-1] + prices[i]);
            result = max(result,sell[i]);
        }
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 42% python ??%** 
![](/assets/img/sample/leetcode/122/result.JPG)  






