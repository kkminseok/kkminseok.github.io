---
title: leetcode(리트코드)207-Course Schedule
author: 강민석
date: 2021-03-26 13:01:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 207 - Course Schedule 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/course-schedule/>  

![](/assets/img/sample/leetcode/207/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/207/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- prerequisites에 있는 값의 첫번째 값은 강의 번호이고, 두번째 값은 먼저 들어야할 강의 입니다.
- 병목현상이 일어나면 false를 병목현상이 일어나지 않고 강의를 들을 수 있으면 true를 리턴합니다.
- 방문처리를 좀 더 잘해주면 빠른 코드가 나왔을텐데 아쉽습니다.



-----  

## 5. code


**c++**


```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int>* graph = new vector<int>[numCourses+1];
        bool* check = new bool[sizeof(bool) * numCourses+1];
        for(size_t i =0;i<prerequisites.size();++i)
        {
            graph[prerequisites[i][0]].push_back(prerequisites[i][1]);
        }
        
        //BFS
        for(int i = 0;i<numCourses;++i)
        {
            if(graph[i].size()==0)
                continue;
            else
            {
                fill(check,check+numCourses+1,false);
                queue<int> q;
                q.push(i);
                check[i]=true;
                while(!q.empty())
                {
                    int x = q.front();
                    //cout<<x<<'\n';
                    q.pop();
                    for(int k =0;k<graph[x].size();++k)
                    {
                        int temp = graph[x][k];
                        if(graph[temp].size()==0)
                            continue;
                        if(!check[graph[x][k]])
                        {
                            q.push(graph[x][k]);
                            check[graph[x][k]]=true;
                        }
                        if(graph[x][k]==i)
                        {
                            return false;
                        }
                    }
                }
                
            }
        }
        return true;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ ..% python ??%** 
![](/assets/img/sample/leetcode/207/result.JPG)  


**빠른코드 c++**

```c++
public:
    bool canFinish(int numCourses, vector<vector<int>> &prerequisites)
    {

        m_graph = vector<vector<int>>(numCourses);

        for (const auto &p : prerequisites)
            m_graph[p[0]].push_back(p[1]);

        // states: 0 = unkonwn, 1 == visiting, 2 = visited
        vector<int> v(numCourses, 0);

        for (int i = 0; i < numCourses; ++i)
            if (dfs(i, v))
                return false;

        return true;
    }

private:
    vector<vector<int>> m_graph;
    bool dfs(int cur, vector<int> &v)
    {
        if (v[cur] == 1)
            return true;
        if (v[cur] == 2)
            return false;

        v[cur] = 1;

        for (const int t : m_graph[cur])
            if (dfs(t, v))
                return true;

        v[cur] = 2;

        return false;
    }
};
```

DFS를 돌면서 1과 2로 방문처리를 해주는 코드입니다.

