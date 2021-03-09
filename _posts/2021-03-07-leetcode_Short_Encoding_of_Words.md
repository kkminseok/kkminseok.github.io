---
title: leetcode(리트코드)3월06일 challenge820-Short Encoding of Words
author: 강민석
date: 2021-03-07 00:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 06일 - Short Encoding of Words 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/short-encoding-of-words/>  

![](/assets/img/sample/leetcode/820/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/820/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 06일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 문제 해석을 못 했습니다..그래서 discuss를 봤습니다.



-----  

## 5. code

```c++
class Solution {
public:
        int minimumLengthEncoding(vector<string>& words) {
        unordered_set<string> s(words.begin(), words.end());
        for (string w : s)
            for (int i = 1; i < w.size(); ++i)
                s.erase(w.substr(i));
        int res = 0;
        for (string w : s) res += w.size() + 1;
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

Discuss를 봤으므로 올리지 않습니다.



