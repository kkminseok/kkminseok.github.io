---
title: leetcode(리트코드)2월1일 challenge191-Number Of 1 Bits
author: 강민석
date: 2021-02-11 14:06:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge01 - Number Of 1 Bits문제입니다.**

## 1. 문제
<https://leetcode.com/explore/challenge/card/february-leetcoding-challenge-2021/585/week-2-february-8th-february-14th/3634/>  

![](/assets/img/sample/leetcode/191/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/191/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월1일자 챌린지 문제입니다. 
leetcode를 2월 9일부터 시작해서 늦게나마 풉니다.  

-----  

## 4. 문제 해석

- 인풋으로 들어오는 최대 32의 길이를 갖는 값의 1의 갯수를 세줘야합니다.  


-----  

## 5. code

```c++

class Solution {
public:
    int hammingWeight(uint32_t n) {
        int result =0;
        while(n!=0)
        {
            if(n%2==1)
                ++result;
            n/=2;
        }
        return result;
    }
};

```
-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/191/result.JPG) 


**시간효율성이 좋은 코드**

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        if(n==0)
            return 0;
        else
        return (n&1) + hammingWeight(n>>1);
            
    }
};
```

1011로 들어온 경우 마지막 비트와 1을 &연산(둘 다 1인 경우 1을 반환)을 합니다. 그리고 1씩 shift 연산을하여 똑같은 &연산을 해줍니다.  

```console
  1011
&    1
= 1
   101
&    1
= 1
    10
&    1
= 0
...

```