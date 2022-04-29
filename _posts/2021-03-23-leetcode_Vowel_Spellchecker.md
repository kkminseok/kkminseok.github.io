---
title: leetcode(리트코드)3월22일 challenge966-Vowel Spellchecker
author: 강민석
date: 2021-03-23 00:03:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 22일 - Vowel Spellchecker 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/vowel-spellchecker/>  

![](/assets/img/sample/leetcode/966/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/966/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 22일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- wordlist가 들어오고 queries가 들어옵니다.
- queries안에 있는 문자열이 wordlist에 있으면 냅두고, 대소문자만 다르면 wordlist의 앞에서부터 가깝게 위치한 문자열을 리턴, 'a', 'e', 'o', 'u', 'i'로 치환이 가능하면 해당 문자열을 리턴 없으면 ""를 리턴합니다.

- discuss로 보고 아이디어를 얻어서 풀었습니다.


-----  

## 5. code

**c++**

```c++
class Solution {
public:
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        //중복처리를 위한.
        unordered_set<string> words(wordlist.begin(),wordlist.end());
        //lowlist는 key값으로 문자열을 소문자로 치환한 값, value값으로 원본 문자열이 들어갑니다.
        //vowlist는 key값으로 'a','e','o','u','i'를 '#'로 치환한 값, value는 원본 문자열이 들어갑니다.
        unordered_map<string,string> lowerlist,vowlist;
        for(string w : wordlist){
            string lowword = makelow(w),  vowword = makevow(w);
            lowerlist.insert({lowword,w});
            vowlist.insert({vowword,w});
        }
        
        for(size_t i =0;i<queries.size();++i)
        {
            if(words.count(queries[i]))continue;
            string lowq = makelow(queries[i]), vowq = makevow(queries[i]);
            if(lowerlist.count(lowq))
                queries[i] = lowerlist[lowq];
            else if(vowlist.count(vowq))
                queries[i] = vowlist[vowq];
            else
                queries[i] = "";
        }
        return queries;
    }
    string makelow(string w){
        for(auto& c : w){
            c = tolower(c);
        }
        return w;
    }
    string makevow(string w){
        w= makelow(w);
        for(auto&c : w){
            if(c =='a'||c=='e'||c=='i'||c=='o'||c=='u')
                c='#';
        }
        return w;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 83%**
![](/assets/img/sample/leetcode/966/result.JPG)  

