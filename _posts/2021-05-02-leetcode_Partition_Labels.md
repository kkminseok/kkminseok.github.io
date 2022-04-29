---
title: leetcode(리트코드)763-Partition Labels
author: 강민석
date: 2021-05-02 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 763 - Partition Labels 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/partition-labels/> 

![](/assets/img/sample/leetcode/763/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/763/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.      


-----  

## 4. 문제 해석

- 문자열이 들어옵니다.
- 문자가 최대 한 부분에 나타나게 하여 그 부분 문자열의 길이들을 벡터에 넣고 리턴합니다.



-----  

## 5. code  

코드설명
- 어디까지 나타나는 지에 대한 정보를 입력해야 합니다.


**c++**

```c++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        vector<int> charIdx(26,0);
        for(int i = 0 ; i<S.size();++i){
            charIdx[S[i]-'a'] = i;
        }
        
        int maxIdx = -1, lastIdx = 0 ;
        vector<int> res;
        for(int i = 0 ; i <S.size();++i){
            maxIdx = max(maxIdx,charIdx[S[i]-'a']);
            if(maxIdx == i ){
                res.push_back(maxIdx - lastIdx+1);
                lastIdx = i +1;
            }
        }
        return res;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 81% 4ms**

![](/assets/img/sample/leetcode/763/result.JPG)  



