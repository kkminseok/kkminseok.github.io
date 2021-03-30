---
title: leetcode(리트코드)3월28일 challenge423-Reconstruct Original Digits from English
author: 강민석
date: 2021-03-30 00:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 28일 - Reconstruct Original Digits from English 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reconstruct-original-digits-from-english/>  

![](/assets/img/sample/leetcode/423/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/423/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 28일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 문자열에 있는 흩어진 문자열을 모아 오름차순으로 정렬된 숫자를 리턴합니다.
- 좋은 문제는 아닌게 one,two,three ... 하드코딩을 해야합니다.




-----  

## 5. code

**c++**

```c++
class Solution {
public:
    string originalDigits(string s) {
        int count[10] = {0,};
        for(size_t i =0;i<s.size();++i)
        {
            char stri = s[i];
            if(stri == 'z') count[0]++;
            if (stri == 'w') count[2]++;
            if (stri == 'x') count[6]++;
            if (stri == 's') count[7]++; //7-6
            if (stri == 'g') count[8]++;
            if (stri == 'u') count[4]++; 
            if (stri == 'f') count[5]++; //5-4
            if (stri == 'h') count[3]++; //3-8
            if (stri == 'i') count[9]++; //9-8-5-6
            if (stri == 'o') count[1]++; //1-0-2-4
        }
        count[7]-=count[6];
        count[5] -= count[4];
        count[3] -=count[8];
        count[9] =  count[9] - count[8] - count[5]- count[6];
        count[1] = count[1] -count[0] - count[2]-count[4];
        string result = "";
        for(int i = 0;i<10;++i){
            for(int j =0;j<count[i];++j)
                result+=i+48;
        }
        
        return result;
    }
    
};
```
-----

## 6. 결과 및 후기, 개선점

**c++ 68%**
![](/assets/img/sample/leetcode/423/result.JPG)  


편법으로 푼 문제이지만 모두 편법으로 풀었습니다.