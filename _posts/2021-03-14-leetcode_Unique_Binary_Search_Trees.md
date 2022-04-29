---
title: leetcode(리트코드)96-Unique Binary Search Trees
author: 강민석
date: 2021-03-14 00:02:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 96 - Unique Binary Search Trees 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/unique-binary-search-trees/>  

![](/assets/img/sample/leetcode/96/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/96/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- DP문제입니다. 이런 경우의 수 문제는 규칙이 있을 수 밖에.. 없을 것입니다.
- 점화식을 해석, 생각하는 데 오래걸렸고 생각하기 힘듭니다.
- 만약 n=5라고 하면 {1,2,3,4,5}이렇게 트리의 루트가 될 수 있습니다. '1'를 루트노드라고 하면 왼쪽 자식에는 올 것이 없고 오른쪽 자식에는 {2,3,4,5}를 트리로 만들어야하는데 이 경우의 수는 n=4일 때의 경우의 수와 같습니다. '2'를 루트노드라고 하면 왼쪽 자식에는 {1}, 오른쪽 자식에는 {3,4,5}가 와야하는데 이는 왼쪽의 경우 n=1, 오른쪽 자식은 n=3인 경우의 수입니다. 이런식으로 진행하다보면
따라서 밑과 같은 점화식이 성립하는 것입니다.

-----  

## 5. code

**python**

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp =[0] * 20
        dp[0] = 1
        dp[1] = 1
        for i in range(2,n+1):
            for j in range(1,i+1):
                dp[i] = dp[i] + dp[j-1] * dp[i-j]
        return dp[n]
```

-----  

**c++**

```c++
class Solution {
public:
    int numTrees(int n) {
        int dp[20]={0,};
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n ;++i)
            for(int j = 1;j<=i;++j)
                dp[i] += (dp[j-1] * dp[i-j]);
        return dp[n];
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 100% python 84%**  
![](/assets/img/sample/leetcode/96/result.JPG)  

 
