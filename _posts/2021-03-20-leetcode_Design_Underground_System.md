---
title: leetcode(리트코드)3월20일 challenge1396-Design Underground System
author: 강민석
date: 2021-03-20 09:00:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 20일 - Design Underground System 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/design-underground-system/>  

![](/assets/img/sample/leetcode/1396/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1396/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 20일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- checkIn으로 고유한 ID, 시작 장소, 도착한 시간이 들어옵니다.
- checkout으로 고유한 ID, 끝나는 장소, 나간시간이 들어옵니다.
- getAverageTime으로 시작한 장소와 끝나는 장소가 들어오고 그 장소에서 사람들이 평균적으로 머무른 시간 (나간시간 - 도착한시간) / 사람수 를 리턴합니다.
- map2개와 pair 2개를 써서 관리하였습니다. 

-----  

## 5. code

**c++**

```c++
class UndergroundSystem {
    //stationName, id, time
    unordered_map<int,pair<string,int>> inmm;
    unordered_map<string,pair<int,int>> outmm;
public:
    UndergroundSystem() {
        
    }
    
    void checkIn(int id, string stationName, int t) {
        inmm[id]= {stationName , t};
    }
    
    void checkOut(int id, string stationName, int t) {
        auto& it = inmm[id];
        string end = it.first + "-" + stationName;
        outmm[end].first += t - it.second;
        outmm[end].second +=1;
        
    }
    
    double getAverageTime(string startStation, string endStation) {
        string find = startStation + "-" + endStation;
        double result = double(outmm[find].first) / double(outmm[find].second);
        return result;
    }
};

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem* obj = new UndergroundSystem();
 * obj->checkIn(id,stationName,t);
 * obj->checkOut(id,stationName,t);
 * double param_3 = obj->getAverageTime(startStation,endStation);
 */
 ```

-----

## 6. 결과 및 후기, 개선점

**c++ 78%**


![](/assets/img/sample/leetcode/1396/result.JPG)  

