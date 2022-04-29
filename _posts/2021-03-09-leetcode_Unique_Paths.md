---
title: leetcode(리트코드)62-Unique Paths
author: 강민석
date: 2021-03-09 06:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 62 - Unique Paths 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/unique-paths/>  

![](/assets/img/sample/leetcode/62/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/62/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- MAP의 길이가 주어집니다. MAP의 끝에 도달하는 경우의 수를 리턴합니다.
- 옛날 수학시간에 배운 길찾기 (위에서 오는 경우의 수 + 왼쪽에서 오는 경우의 수)를 통해 풀었습니다.



-----  

## 5. code

**python**

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        v = []
        for i in range(m) : 
            v.append([])
            for j in range(n) :
                v[i].append([])
        for i in range(m):
            for j in range(n):
                if i == 0 or j ==0 : 
                    v[i][j] = 1 
                else : 
                    v[i][j] = v[i-1][j] + v[i][j-1]
        return v[m-1][n-1]
```

-----  

**c++**

```c++
class Solution {
public:
    void cal(int m,int n,int** map)
    {
        for(int x =1;x < m ;++x)   
        {
            for(int y = 1; y<n;++y)
            {
                map[x][y] = map[x-1][y] + map[x][y-1];
            }
        }
    }
    
    int uniquePaths(int m, int n) {
        int** map = new int*[m];
        for(int i =0;i<m;++i)
            map[i] = new int[n];
        for(int i=0;i<m;++i)
        {
            for(int j =0;j<n;++j)
            {
                map[i][j]=1;
            }
        }
        cal(m,n,map);
        int result = map[m-1][n-1];
        for(int i =0;i<m;++i)
        {
            delete []map[i];
        }
        delete[] map;
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

c++ 100%

![](/assets/img/sample/leetcode/62/result.JPG)  



**python100% 12ms code**

```python
class Solution:
    def __init__(self):
        self.dp = {} 
    def uniquePaths(self, m: int, n: int) -> int:
        #이미 돌았으면 값을 리턴
        if (m, n) in self.dp:
            return self.dp[(m, n)]
        
        if m == 1 or n == 1:
            return 1
        #재귀문
        ans = self.uniquePaths(m, n-1) + self.uniquePaths(m-1, n)
        self.dp[(m, n)] = ans
        
        return ans
```