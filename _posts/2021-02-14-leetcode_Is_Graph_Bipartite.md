---
title: leetcode(리트코드)2월14일 challenge785-Is Graph Bipartite?
author: 강민석
date: 2021-02-14 17:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge14 - Is Graph Bipartite ? 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/is-graph-bipartite/>  

![](/assets/img/sample/leetcode/785/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/785/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월14일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 간단하게 이분 그래프 판별 문제입니다. 백준에 똑같은 문제가 있습니다.
<https://kkminseok.github.io/posts/baekjoon1707/>


-----  

## 5. code

```c++
int MAP[101]={0,};
class Solution {
public:
    bool BFS(int start, vector<vector<int>>& graph)
    {
        queue<int> q;
        q.push(start);
        MAP[start] = 1;
        while (!q.empty())
        {
            int curr = q.front();
            q.pop();
            for (int i = 0; i < graph[curr].size(); ++i)
            {
                int ver = graph[curr][i];
                if (MAP[ver] != 0)//방문기록이 있음.
                {
                    if (MAP[curr] == MAP[ver])//색이 같다.
                    {
                        return false;
                    }
                }
                else
                {
                    q.push(ver);
                    MAP[ver] = 3 - MAP[curr];
                }
            }

        }
        return true;
    }

    
    
    bool isBipartite(vector<vector<int>>& graph) {
        bool result = true;
        memset(MAP,0,sizeof(MAP));
        for(size_t i =0;i<graph.size();++i)
        {
            if(MAP[i]==0)
            {
                result = BFS(i,graph);
                cout<<i<<" "<<result<<'\n';
                if(result==false)
                    return result;
                
            }
        }
        
        return result;
    }
};
```
-----

## 6. 결과 및 후기, 개선점
  
**시간(96%)**
![](/assets/img/sample/leetcode/785/result.JPG) 

