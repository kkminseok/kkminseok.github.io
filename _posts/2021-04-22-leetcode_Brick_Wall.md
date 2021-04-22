---
title: leetcode(리트코드)4월22일 challenge554-Brick Wall
author: 강민석
date: 2021-04-22 10:17:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 22일 - Brick Wall 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/brick-wall/>  

![](/assets/img/sample/leetcode/554/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/554/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
4월 22일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- Brick이라는 2차원 벡터가 주어집니다.
- 각 배열의 요소는 Brick의 넓이로 수직선을 그었을 때 최소한의 Brick을 지나는 경우의 Brick수를 리턴합니다.
- Brick의 넓이는 최대 INT_MAX까지 들어옵니다. 또한, 각 지점의 끝을 수직으로 긋는 것은 안됩니다. 그럴 수 밖에 없는 경우 높이를 리턴하면됩니다.




-----  

## 5. code

- 핵심기능  
핵심적인 코드 및 생각해야할 것은 Brick의 폭을 더해 놓으면 경계선이 저장되므로 그 값을 이용해서 문제를 해결해나가면 됩니다.

**c++**

```c++
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<int,int> um;
        int maxcol = 0;
        int res = wall.size();
        int height = res;
        for(int i = 0 ;i<height; ++ i){
            int sum = 0;
            for(int j = 0 ; j <wall[i].size();++j){
                //경계선을 계산.
                sum += wall[i][j];
                um[sum]++;
            }
            maxcol = sum;
        }
        for(auto it = um.begin(); it!=um.end();++it){
            //끝인 경우는 continue
            if(it->first == maxcol) continue;
            // 높이에서 경계선을 가진 row의 개수를 빼줍니다.
            res = min(res, height- it->second);
        }
        return res;
    }
};

```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/554/result.JPG)  




