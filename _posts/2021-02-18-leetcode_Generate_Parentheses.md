---
title: leetcode(리트코드)22-Generate Parentheses
author: 강민석
date: 2021-02-18 16:10:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 22 - Generate Parentheses 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/generate-parentheses/>  

![](/assets/img/sample/leetcode/22/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/22/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- n으로 들어온 값으로 만들 수 있는 "()"짝을 만들어 리턴합니다.
- 재귀로 푸는 방식은 알았으나 해결하지 못하여 Discuss를 봤습니다.




-----  

## 5. code

```c++
class Solution {
public:
    vector<string> result;
    void solution(int head,int tail,int n,string temp)
    {
        if(temp.size()==n*2 )
        {
            result.push_back(temp);
        }
        else
        {
            if(head<n)
                solution(head+1,tail,n,temp+'(');
            if(tail<head)
                solution(head,tail+1,n,temp+')');   
        }
    }
    vector<string> generateParenthesis(int n) {;
        solution(0,0,n,"");
        
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

Discuss를 봤으므로 올리지 않습니다.





