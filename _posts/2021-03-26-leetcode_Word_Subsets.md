---
title: leetcode(리트코드)3월26일 challenge916-Word Subsets
author: 강민석
date: 2021-03-26 12:34:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 26일 - Word Subsets 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/word-subsets/>  

![](/assets/img/sample/leetcode/916/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/916/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 26일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- B로 들어온 문자열을 모두 가지고 있는 A의 문자열을 찾아 vector에 넣고 반환합니다.




-----  

## 5. code

**c++**

```c++
class Solution {
public:
    vector<string> wordSubsets(vector<string>& A, vector<string>& B) {
        //B로들어온 문자열들을 합칠 것입니다.
        string temp = "";
        //합치기 위한 사전 작업. 'oo'와 같이 2개의 하나의 문자열 안에 동일한 문자가 2개 이상 포함될 경우 처리해주어야 합니다.
        int chrcount[26]={0,};
        for(size_t i =0;i<B.size();++i)
        {
            int subcount[26]={0,};
            for(size_t j =0;j<B[i].size();++j)
            {
                subcount[B[i][j]-97]++;
                chrcount[B[i][j]-97] = max(subcount[B[i][j]-97], chrcount[B[i][j]-97]);
            }
        }
        //저장된 값들을 이용해서 새로운 문자열을 만듭니다.
        for(int i =0;i<26;++i)
        {
            while(chrcount[i]!=0)       
            {
                temp+=(i+97);
                chrcount[i]--;
            }
        }
        //결과 벡터
        vector<string> result;
        //한번에 비교하기 위해 비교할 두 문자열 모두 정렬합니다.
        sort(temp.begin(),temp.end());
        for(size_t i =0;i<A.size();++i)
        {
            bool check=true;
            string Atemp = A[i];
            sort(Atemp.begin(),Atemp.end());
            //subindex는 Atemp를 돌 문자열 j는 temp문자열을 돌 인덱스입니다.
            int subindex = 0;
            int j = 0;
            for(;j<temp.size() && subindex<Atemp.size();)
            {
                if(Atemp[subindex] == temp[j])
                {
                    ++subindex;
                    ++j;
                }
                else
                    subindex++;
            }
            //j가 temp.size() 라는 것은 temp를 다 돌았다는 것이므로 A[i]안에 B가 포함되어 있다는 뜻입니다.
            if(j==temp.size())
                result.push_back(A[i]);
        }

        return result;
    }
    
};
```
-----

## 6. 결과 및 후기, 개선점

**c++ 86%**
![](/assets/img/sample/leetcode/916/result.JPG)  

