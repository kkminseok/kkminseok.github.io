---
title: leetcode(리트코드)2월22일 challenge524-Longest Word in Dictionary through Deleting
author: 강민석
date: 2021-02-22 00:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February 524 - Longest Word in Dictionary through Deleting 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/>  

![](/assets/img/sample/leetcode/524/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/524/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월22일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- s에 있는 문자를 빼서 dictionary에 있는 문자열을 만들 수 있으면 됩니다. 리턴할 문자열은 가장 길이가 길어야합니다.
- 리턴할 문자열의 길이가 같을 땐 사전순으로 빠른 것을 리턴합니다.

-----  

## 5. code

```c++
class Solution {
public:
    
    string findLongestWord(string s, vector<string>& dictionary) {
        //결과 문자열
        string result = "";
        //길이가 큰 문자열을 담을 변수
        int sz= 0;
        //길이가 같으면 사전 순으로 처리해야하므로 정렬을합니다.
        sort(dictionary.begin(),dictionary.end());
        for(size_t i =0;i<dictionary.size();++i)
        {
            //index는 사전에 있는 문자열을 돌 것입니다.
            int index = 0;
            //예를 들어 apple를 돌고 plea를 돈다하면 plea는 apple보다 문자열의 크기가 작으므로 돌 필요가 없습니다.
            if(dictionary[i].size()>sz)
            {
                for(size_t j = 0; j<s.size();++j)
                {
                    if(dictionary[i][index]==s[j])
                    {
                        ++index;
                    }
                    if(index == dictionary[i].size())
                    {
                        sz = index;
                        result = dictionary[i];
                    }
                }
            }
        }
        return result;
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/524/result.JPG)  


**시간이 빠른 코드 24ms(100%)**

```c++
class Solution {
public:
    
    inline bool contains(string &s, string &t)
    {
        int n = s.length(),
            m = t.length();
        int i = 0 , j = 0;
        while(i<n and j<m)
            if(s[i] == t[j])
                i++ , j++;
            else
                i++;
        return j == m;
    }
    
    string findLongestWord(string s, vector<string>& d) {
        //입출력을 빠르게
        ios_base::sync_with_stdio(0),cin.tie(0);
        
        //함수 포인터 d(dictionary)를 길이 순으로 정렬
        //같은 길이면 사전순으로 정렬.
        //나머지 로직은 같다.
        auto cmp = [](string &a, string &b)
        {
            if(a.length() != b.length())
                return a.length() > b.length();
            return a<b;
        };
        sort(d.begin() , d.end() , cmp);
        
        for(int i=0 ; i<d.size() ; i++)
        {
            if(contains(s,d[i]))
                return d[i];
        }
        return "";
    }
};
```