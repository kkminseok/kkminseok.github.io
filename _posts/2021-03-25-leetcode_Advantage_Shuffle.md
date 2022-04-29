---
title: leetcode(리트코드)3월24일 challenge870-Advantage Shuffle
author: 강민석
date: 2021-03-25 00:03:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 24일 - Advantage Shuffle 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/advantage-shuffle/>  

![](/assets/img/sample/leetcode/870/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/870/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 24일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- A[i] > B[i]로 만들 수 있게 A의 값들을 바꿔야합니다. A[i] > B[i] 인 갯수가 최대가 되도록 바꿔야합니다.



-----  

## 5. code

**c++**

```c++
bool pred(pair<int,int>& a,pair<int,int>& b)
{
    if(a.first==b.first)
        return a.second<b.second;
    return a.first<b.first;
}
class Solution {
public:
    vector<int> advantageCount(vector<int>& A, vector<int>& B) {
        //tempB는 B의 값과 인덱스를 저장합니다. 정렬을 해줄 것이므로.
        vector<pair<int,int>> tempB;
        //배열의 끝을 돌 때까지 정렬하지 못한 것은 queue에 넣습니다.
        queue<int> q;
        //결과 벡터입니다.
        vector<int> result(A.size());
        for(size_t i =0;i<B.size();++i)
            tempB.push_back(make_pair(B[i],i));
        //B의 인덱스와 값을 넣은 tempB를 정렬합니다.
        sort(tempB.begin(),tempB.end(),pred);
        sort(A.begin(),A.end());

        //j는 tempB를 도는 인덱스 값입니다.
        int j = 0;
        for(size_t i = 0;i<A.size();++i)
        {
            int value = tempB[j].first;
            int index = tempB[j].second;
            //만약 A[i]가 조건에 맞지 않으면 큐에 넣습니다.
            if(A[i]<=value)
            {
                q.push(A[i]);
                continue;
            }
            //조건에 맞으므로 result벡터에 값을 넣어줍니다.
            result[index] = A[i];
            ++j;
        }
        //q가 빌 때까지 접근하지 않은 인덱스에 값들을 넣어줍니다.
        while(!q.empty())
        {
            for(size_t i =0;i<result.size();++i)
            {
                if(result[i]==0)
                {
                    result[i] =q.front();
                    q.pop();
                }
            }
        }
        
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 92%**
![](/assets/img/sample/leetcode/870/result.JPG)  

