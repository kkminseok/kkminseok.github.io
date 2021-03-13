---
title: leetcode(리트코드)3월13일 challenge823-Binary Trees With Factors
author: 강민석
date: 2021-03-13 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 13일 - Binary Trees With Factors 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/binary-trees-with-factors/>  

![](/assets/img/sample/leetcode/823/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/823/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 13일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- arr에 들어있는 숫자로 만들 수 있는 트리의 개수를 셉니다.
- 트리의 조건은 양쪽의 자식 노드를 곱해서 부모노드를 만들 수 있으면 됩니다.
- DP로 풀 수 있습니다. 왜냐하면 자식 노드를 곱해서 부모노드를 만들 수 있으므로 사실상 두 자식 노드를 만들 수 있는 경우의 수를 더하면 그 부모노드를 만들 수 있는 경우의 수가 되기 때문입니다.





-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int numFactoredBinaryTrees(vector<int>& arr) {
        int mod = pow(10,9) + 7;
        sort(arr.begin(),arr.end());
        unordered_map<int,long> dp;
        long result = 0;
        for(size_t i =0;i<arr.size();++i)
        {
            dp[arr[i]] = 1;
            for(int j = 0 ; j<i;++j)
            {
                if(arr[i] % arr[j] == 0)
                    dp[arr[i]] = (dp[arr[i]] + dp[arr[j]] * dp[arr[i] /arr[j]]) %mod;
            }
            result =  (result + dp[arr[i]])%mod;
        }
        return result;
    }
};


```

-----

**python**

```python
class Solution:
    def numFactoredBinaryTrees(self, arr: List[int]) -> int:
        dp ={}
        result = 0
        mod = pow(10,9) + 7
        arr.sort()
        
        for i in range(len(arr)):
            dp[arr[i]] = 1
            for j in range(i):
                if arr[i] % arr[j] == 0:
                    if(arr[i]//arr[j]) in dp : 
                        dp[arr[i]] = (dp[arr[i]] + dp[arr[j]] *(dp[arr[i] // arr[j]])) %mod
                    else:
                        dp[arr[i]//arr[j]] =0
            result = (result+ dp[arr[i]]) %mod
        
        return result
```

-----

## 6. 결과 및 후기, 개선점

**c++ 40% python 50%**

![](/assets/img/sample/leetcode/823/result.JPG)  
