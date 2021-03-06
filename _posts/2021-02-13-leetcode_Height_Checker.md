---
title: leetcode(리트코드)1051-Height Checker
author: 강민석
date: 2021-02-13 12:30:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Height Checker 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/height-checker/>  

![](/assets/img/sample/leetcode/1051/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1051/input.JPG)

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 값을 정렬하는데 정렬하는 데 필요한 위치 변동의 최소값을 구하는 문제입니다. 
- 문제 해석이 잘 못 된건지 해석하기 어려운 점이 있습니다.
- 문제에서 예시로 들어주는 [1,1,4,2,1,3]이 최소 2번으로 정렬이 가능하는데 문제에서 요구하는 답은 3입니다.  

```console
    [1,1,4,2,1,3]
->  [1,1,1,2,4,3]  1
->  [1,1,1,2,3,4]  2
```

그렇기에 Bad에 투표가 많고, Discuss에서 이 문제에 대한 논의가 계속 되고 있습니다.  


-----  

## 5. code

```c++
class Solution {
public:
    int heightChecker(vector<int>& h, int res = 0) {
        vector<int> s = h;
        sort(begin(s), end(s));
        for (auto i = 0; i < h.size(); ++i) res += h[i] != s[i];
        return res;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/1051/result.JPG)  






