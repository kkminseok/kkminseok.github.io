---
title: leetcode(리트코드)2월16일 challenge784-Letter Case Permutation
author: 강민석
date: 2021-02-16 16:40:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge16 - Letter Case Permutation 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/letter-case-permutation/>  

![](/assets/img/sample/leetcode/784/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/784/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월16일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 만들 수 있는 문자열을 전부 찾아 리턴합니다. 대문자 소문자가 들어오면 두가지 경우의 수를 모두 넣어주고 숫자다 들어올 경우 무시하며 넘어갑니다.
- 재귀로 작성하였습니다.



-----  

## 5. code

```c++
class Solution {
public:
    void cal(vector<string>& result,string S,int index,string temp)
    {
        //만약 끝의 인덱스에 도달했으면 벡터에 넣어줍니다.
        if(index==S.size())
        {
            //cout<<temp;
            result.push_back(temp);
            return;
        }
        else
        {
            //digit
            if(S[index]>=48 && S[index]<=57)
                cal(result,S,index+1,temp+=S[index]);
            else
            {
                //tolower는 소문자로 만들어줍니다. 소문자로 만들고 temp에 추가한 뒤 넘겨줍니다.
                cal(result,S,index+1,temp+=(tolower(S[index])));
                //마지막 요소를 제거합니다. 이유는 temp가 위에서 저장된채로 밑으로 넘어가면 aAbB12 이런식으로 됩니다. 원하는 것은 ab12 aB12입니다.
                temp.resize(temp.size()-1);
                cal(result,S,index+1,temp+=(toupper(S[index])));
            }
        }
    }
    
    vector<string> letterCasePermutation(string S) {
        //결과를 담을 변수입니다.
        vector<string> result;
        //인덱스 하나하나 접근해서 담을 임시 변수입니다.
        string temp="";
        cal(result,S,0,temp);
        return result;
        
    }
};
```



-----

## 6. 결과 및 후기, 개선점
  
**시간(95%)**
![](/assets/img/sample/leetcode/784/result.JPG) 



