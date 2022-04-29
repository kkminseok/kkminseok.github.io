---
title: leetcode(리트코드)2월26일 challenge946-Validate Stack Sequences
author: 강민석
date: 2021-02-26 14:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February 946 - Validate Stack Sequences 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/validate-stack-sequences/>  

![](/assets/img/sample/leetcode/946/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/946/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월26일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- pushed로 된 벡터와 popped으로 된 벡터가 있습니다. 주어진 순서대로 push를 하고 임의의 pop을 통해 popped 벡터를 만들 수 있는 지 확인합니다.

-----  

## 5. code

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> st;
        int pi=0;
        int popi=0;
        for(;popi<popped.size();)
        {
            //처음 접근하면 밑의 st.top()연산을 할 수 없으모로 넣어줍ㄴ디ㅏ.
            if(st.empty())
                st.push(pushed[pi++]);
            while(st.top()!=popped[popi] && pi<pushed.size())
            {
                st.push(pushed[pi++]);
            }
            //만약 push배열에 있는 모든 걸 넣었는데 다르다면 false를 리턴합니다.
            if(st.top()!=popped[popi])
                return false;
            st.pop();
            ++popi;
        }
        return true;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


![](/assets/img/sample/leetcode/946/result.JPG)  

