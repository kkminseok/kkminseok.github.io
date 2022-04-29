---
title: leetcode(리트코드)3월11일 challenge322-Coin Change
author: 강민석
date: 2021-03-11 05:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 11일 - Coin Change 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/coin-change/>  

![](/assets/img/sample/leetcode/322/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/322/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 11일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 대표적인 DP문제입니다. 주어진 coins을 최소한 사용하여 amount를 만들어야합니다. 만들 수 없으면 -1을 리턴합니다.



-----  

## 5. code

**python**

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        coins.sort()
        dp = [987654] * (amount+1)
        dp[0] = 0
        for i in range(1,amount+1):
            for j in range(len(coins)):
                if coins[j] <= i:
                    dp[i] = min(dp[i],dp[i-coins[j]]+1)
        return -1 if dp[amount] == 987654 else dp[amount]
```

-----  

**c++**

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        sort(coins.begin(),coins.end());
        int* dp = new int[amount+1];
        dp[0]=0;
        for(int i =1;i<amount+1;++i)
            dp[i]= 987654;
        
        for(int i =1;i<=amount; ++i)
        {
            for(int j = 0; j<coins.size();++j)
            {
                if(coins[j]<=i)
                    dp[i] = min(dp[i], dp[i -coins[j]] +1 );
            }
        }
        return dp[amount] == 987654 ? -1 : dp[amount];
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 94% python 40%**

![](/assets/img/sample/leetcode/322/result.JPG)  

python 개선한 코드들이 너무 보기 어려워서 추가하지 않겠습니다.

 


