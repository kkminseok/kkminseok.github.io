---
title: leetcode(리트코드)3월10일 challenge12-Integer to Roman
author: 강민석
date: 2021-03-10 08:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 10일 - Integer to Roman 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/integer-to-roman/>  

![](/assets/img/sample/leetcode/12/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/12/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 10일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- Bad가 더 많은 좋지 않은 문제입니다. 이유는 하드코딩식으로 풀어야합니다. 심지어 시간이 빠른 코드도 하드코딩식으로 풀었습니다.
- 위의 이유에서 python으로는 풀지 않았습니다.

-----  

## 5. code

**c++**

```c++
class Solution {
public:
    string intToRoman(int num) {
        string temp ="";
        while(num!=0)
        {
            if(num>=1000)
            {
                num-=1000;
                temp+='M';
            }
            else if(num>=900)
            {
                num-=900;
                temp+="CM";
            }
            else if(num>=500)
            {
                num-=500;
                temp+='D';
            }
            else if(num>=400)
            {
                num-=400;
                temp+="CD";
            }
            else if(num>=100)
            {
                num-=100;
                temp+='C';
            }
            else if(num>=90)
            {
                num-=90;
                temp+="XC";
            }
            else if(num>=50)
            {
                num-=50;
                temp+='L';
            }
            else if(num>=40)
            {
                num-=40;
                temp+="XL";
            }
            else if(num>=10)
            {
                num-=10;
                temp+='X';
            }
            else if(num>=9)
            {
                num-=9;
                temp+="IX";
            }
            else if(num>=5)
            {
                num-=5;
                temp+='V';
            }
            else if(num>=4)
            {
                num-=4;
                temp+="IV";
            }
            else if(num>0)
            {
                num-=1;
                temp+='I';
            }
        }
        

        
        return temp;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/12/result.JPG)  




