---
title: leetcode(리트코드)2월05일 challenge71-Simplify Path
author: 강민석
date: 2021-02-14 17:10:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge05 - Simplify Path 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/simplify-path/>  

![](/assets/img/sample/leetcode/71/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/71/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월05일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 주어진 규칙에 맞게 path를 바꿔 return해야합니다.
- 영어에 약해서 처리하지 못할 예외가 많아질까봐 설명을 보고 공부했습니다.


-----  

## 5. code

```c++
class Solution {
public:
    string simplifyPath(string path) {
        //result는 결과
        //tmp는 부분 문자열을 담기위해
        string result, tmp;
        //tmp를 저장할 벡터
        vector<string> tmpvec;
        //문자열을 자를 수 있게 도와줍니다.
        stringstream str(path);
        // '/'을 기준으로 잘브니다.
        while(getline(str,tmp,'/'))
        {
            //.이나 공백이 들어올 경우 무시합니다.
            if(tmp =="." || tmp=="") continue;
            //".."이 들어올 경우 만약 tmp저장벡터에 무언가 있으면 빼버립니다.
            if(tmp==".." && !tmpvec.empty()) tmpvec.pop_back();
            //"..이 들어온게 아니라면 벡터에 넣습니다.
            else if(tmp!="..") tmpvec.push_back(tmp);
        }
        //벡터를 끄집어내면서 '/'를 붙여주면서 결과에 넣어줍니다.
        for(auto i : tmpvec)
        {
            result+= ("/" + i );
        }
        //만약 결과가 비어있다면 '/'를 출력합니다.
        if(result.empty())
        {
            result +='/';
        }
        return result;
    }
};
```
-----

## 6. 결과 및 후기, 개선점
  
**시간**  
솔루션을 보고 풀었기에 x

