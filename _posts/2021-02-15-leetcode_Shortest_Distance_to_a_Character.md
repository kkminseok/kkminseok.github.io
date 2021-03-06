---
title: leetcode(리트코드)2월07일 challenge821-Shortest Distance to a Character
author: 강민석
date: 2021-02-15 16:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge07 - Shortest Distance to a character 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/shortest-distance-to-a-character/>  

![](/assets/img/sample/leetcode/821/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/821/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월07일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- c로 들어온 문자를 기준으로 각 인덱스마다 가장 가까운 c를 찾고 거리를 벡터에 넣어 반환합니다.  


-----  

## 5. code

```c++
class Solution {
public:
    //들어온 문자의 위치를 담는 벡터
    vector<int> findindex;
    //절대값으로 변환하는 함수
    int abs(int x)
    {
        return x <0 ? -x : x;
    }
    vector<int> shortestToChar(string s, char c) {
        //결과 벡터
        vector<int> result;
        //문자열을 돌면서 c와 같으면 문자의 위치를 담습니다.
        for(size_t i =0;i<s.size();++i)   
        {
             if(s[i]==c)
                 findindex.push_back(i);
        }
        int i = 0;
        int temp;
        //문자열을 다 돌때까지
        for(int j = 0; j<s.size() && i<findindex.size();++j)
        {
            //i+1을 접근해야합니다. 범위를 넘지않는 한에서 검사를 하고, 만약 다음 문자열의 위치가 더 작아지는 순간이 오면 다음 문자열의 위치를 가리키게 합니다.
            if(i+1 <findindex.size() &&  abs(findindex[i]-j)>abs(findindex[i+1]-j) )
                ++i;
            //분기를 나누는 이유는 i+1이 인덱스 범위를 벗어나는 경우가 있기에 처리를 해줬습니다.
            if(i+1<findindex.size())
                temp= min(abs(findindex[i]-j),abs(findindex[i+1]-j));
            else
                temp = abs(findindex[i]-j);
            result.push_back(temp);
        }
        
        return result;
    }
    
};
```
-----

## 6. 결과 및 후기, 개선점
  
**시간(57%)**
![](/assets/img/sample/leetcode/821/result.JPG) 

**0ms 코드(100%)**

```c++
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        int N = s.length();
        vector<int> ans(N,0);
        int prev =INT_MIN/2;

        for (int i = 0; i < N; ++i) {
            if (s[i] == c)
                prev = i;
            ans[i] = i - prev;
        }

        prev = INT_MAX/2;
        for (int i = N-1; i >= 0; --i) {
            if (s[i] == c)
                prev = i;
            ans[i] = min(ans[i], prev - i);
        }

        return ans;
    }
};
```
위 코드는 한번 i=0부터 끝까지 훑어서 값을 넣고,  
i= N-1(맨 끝)부터 i=0까지 다시 훑어서 작은 값을 넣습니다.  
