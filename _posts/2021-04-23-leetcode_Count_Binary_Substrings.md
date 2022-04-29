---
title: leetcode(리트코드)4월23일 challenge696-Count Binray Substrings
date: 2021-04-23 10:17:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode April 23일 - Count Binary Substrings 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/count-binary-substrings/>  

![](/assets/img/sample/leetcode/696/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/696/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
4월 23일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 0과 1로 구성된 string이 들어옵니다.
- 연속된 부분 스트링에서 0과 1의 갯수가 같은 부분 스트링의 갯수를 반환하세요.





-----  

## 5. code

이전 값을 저장하고 배열의 인덱스가 달라지는 순간 이전값과 현재 값의 카운트 중 작은 값을 찾아 res에 더합니다.

**c++**

```c++
class Solution {
public:
    int countBinarySubstrings(string s) {
        int cur = 1, pre = 0, res = 0;
        for(int i = 1 ; i <s.size(); ++i){
            if(s[i] != s[i-1]){
                res += min(cur,pre);
                pre = cur;
                cur = 1;
            }
            else
                cur++;
        }
        return res += min(pre,cur);
    }
};


```



-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/696/result.JPG)  




