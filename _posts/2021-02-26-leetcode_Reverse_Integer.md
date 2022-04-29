---
title: leetcode(리트코드)7-Reverse Integer
author: 강민석
date: 2021-02-26 12:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 7 - Reverse Integer 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/reverse-integer/>  

![](/assets/img/sample/leetcode/7/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/7/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Interview의 문제입니다.  


-----  

## 4. 문제 해석

- 숫자로 들어온 x 값을 뒤집어서 리턴합니다.
- '싫어요' 갯수가 굉장히 많은데 제 추측으로는 input이 9876543210(2^32보다 큰 값)이 들어올 경우 뒤집으면 int형으로 담을 수 있는 예외처리 때문에 사람들이 문제의도와 안맞다고 생각하는 것 같습니다.

-----  

## 5. code

```c++
class Solution {
public:
    int reverse(int x) {
        if(x==0 || x>2147483647 || x<-2147483647)
            return 0;
        string temp = "";
        bool check = false;
        if(x<0)
        {
            x= -x;
            check = true;
        }
        temp = to_string(x);
        if(check)
            temp+="-";
        std::reverse(temp.begin(),temp.end());
        long long result = stol(temp);
        if(result>INT_MAX || result<INT_MIN)
            return 0;
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/7/result.JPG)  







