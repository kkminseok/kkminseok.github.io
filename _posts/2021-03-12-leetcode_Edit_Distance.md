---
title: leetcode(리트코드)72-Edit Distance
author: 강민석
date: 2021-03-12 01:00:00 +0800
categories: [leetcode,Hard]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 72 - Edit Distance 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/edit-distance/>  

![](/assets/img/sample/leetcode/72/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/72/input.JPG)  

-----  

## 3. 분류 및 난이도

Hard 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- DP문제입니다. word1로 word2로 만드는데 삽입, 삭제, 대체 를 몇 번 이용하여 만들 수 있는지를 리턴합니다.
- DP 점화식이 이해가 안된다면 직접 그리면서 하면 이해가 잘 됩니다.

-----  

## 5. code

**python**

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp=[]
        for i in range(len(word1)+1):
            dp.append([])
            for j in range(len(word2)+1):
                dp[i].append([])
            
        for i in range(len(word1)+1):
            dp[i][0] = i
            for j in range(len(word2)+1):
                dp[0][j] = j
                if i!= 0 and j!= 0 :
                    if word1[i-1] == word2[j-1]:
                        dp[i][j] = dp[i-1][j-1]
                    else : 
                        dp[i][j] = min(dp[i-1][j-1],dp[i-1][j],dp[i][j-1])+1
        return dp[len(word1)][len(word2)]                        
```

-----  

**c++**

```c++
        
class Solution {
public:
    int minDistance(string word1, string word2) {
        int** dp = new int*[word1.size()+1];
        for(int i = 0;i<=word1.size(); ++i)
            dp[i] = new int[word2.size()+1];
        
        for(int i = 0;i<=word1.size();++i)
        {
            dp[i][0] = i;
            for(int j=0;j<=word2.size();++j)
            {
                dp[0][j]= j;
                if(i!=0 && j!=0)
                {
                    if(word1[i-1] == word2[j-1])
                        dp[i][j] = dp[i-1][j-1];
                    else
                        dp[i][j] = min({dp[i-1][j-1], dp[i-1][j],dp[i][j-1]})+1;
                }
            }
        }
     return dp[word1.size()][word2.size()];   
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 68% python 12%**  
![](/assets/img/sample/leetcode/72/result.JPG)  

 
 **c++ 100% 12ms code**

 ```c++
 class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        if(m * n == 0) 
            return m + n;
        
        int DP[m + 1][n + 1];
        for(int i = 0; i <= m; i++)
            DP[i][0] = i;
        for(int i = 0; i <= n; i++)
            DP[0][i] = i;
        
        for(int i = 1; i <= m; i++)
            for(int j = 1; j <= n; j++)
            {
                int top = DP[i - 1][j] + 1;
                int left = DP[i][j - 1] + 1;
                int top_left = DP[i-1][j-1];
                if(word1[i-1] != word2[j-1])
                    top_left++;
                DP[i][j] = min(top,min(left,top_left));
            }
        return DP[m][n];
    }
};
 ```
필요한 값을 미리 꺼내놓고 작업을 하여 속도가 빨라진 것 같습니다.


**python 100% 44ms code**

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        visit, q=set(), [(word1, word2, 0)]
        while True:
            w1, w2, d=q.pop(0)
            if (w1, w2) not in visit:
                if w1==w2:
                    return d
                visit.add((w1, w2))
                while w1 and w2 and w1[0]==w2[0]:
                    w1, w2=w1[1:], w2[1:]
                d+=1
                q.extend([(w1[1:], w2[1:], d), (w1, w2[1:], d), (w1[1:], w2, d)])
```
