---
title: leetcode(리트코드)1408-String Matching in an Array
author: 강민석
date: 2021-04-27 05:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1408 - String Matching in an Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/string-matching-in-an-array/> 

![](/assets/img/sample/leetcode/1408/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1408/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- words라는 string 벡터가 들어옵니다.
- 벡터들의 요소를 비교했을 때 해당 요소가 다른 요소의 substring일 경우 그 요소를 결과 벡터에 넣어 반환하세요.



-----  

## 5. code  

코드설명
- for문 2개를 써서 brute하게 접근합니다. 이유는 최대 100개의 길이를 가진 vector가 들어오므로 O(n^2)이여도 많은 시간이 걸리지 않기 때문입니다.
- 만약 같은 요소를 가리키고 있으면 continue로 스킵해주고 다른 요소에서 substring을 찾았으면 결과벡터에 넣어주고 break합니다.


**c++**

```c++
class Solution {
public:
    vector<string> stringMatching(vector<string>& words) {
        
        set<string> st;
        for(int i = 0 ; i <words.size() ; ++i){
            for(int j = 0 ; j < words.size();++j){
                if(i==j) continue;
                if( words[j].find(words[i]) != -1){
                    st.insert(words[i]);
                    break;
                }
            }
        }
        vector<string> res(st.begin(),st.end());
        return res;    
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 87% 4ms**

![](/assets/img/sample/leetcode/1408/result.JPG)  



