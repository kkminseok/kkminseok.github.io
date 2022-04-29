---
title: leetcode(리트코드)3월16일 challenge714-Best Time to Buy and Sell Stock with Transaction
author: 강민석
date: 2021-03-17 00:03:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 16일 - Best Time to Buy and Sell Stock with Transaction 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/>  

![](/assets/img/sample/leetcode/714/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/714/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 16일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 물건값들들과 세금이 주어집니다. 언제 사고 팔아야 가장 많은 이득을 취할 수 있을까요.?
- 이미 산 물품이 있는데 또 사는건 불가능합니다. 산 물건을 팔아야 다음날 또 살 수 있습니다.
- 비슷한 문제를 백준?에서 풀어봤습니다.

-----  

## 5. code

**c++**


```c++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int* buy = new int[prices.size()];
        int* sell = new int[prices.size()];
        buy[0] = -prices[0];
        sell[0] = 0;
        for(size_t i = 1;i<prices.size();++i)
        {
            buy[i] = max(buy[i-1],sell[i-1] - prices[i]);
            sell[i] = max(sell[i-1], buy[i-1] + prices[i]-fee);
        }

        return sell[prices.size()-1];
    }
};
```

-----  

**python**

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        size = len(prices)
        buy = [0] * size
        sell = [0] * size
        buy[0] = -prices[0]
        sell[0] = 0
        for i in range(1,size):
            buy[i] = max(buy[i-1], sell[i-1] - prices[i])
            sell[i] = max(sell[i-1] , buy[i-1] + prices[i]-fee)
        return sell[size-1]
```

-----

## 6. 결과 및 후기, 개선점

**c++ 96% python 39%**

![](/assets/img/sample/leetcode/714/result.JPG)  


**python 502ms 96%c code**

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        minimum = sys.maxsize
        profit = 0
        
        for p in prices:
            if p < minimum:
                minimum = p
            elif p > minimum + fee:
                profit += p - minimum - fee
                minimum = p - fee
        
        return profit
```