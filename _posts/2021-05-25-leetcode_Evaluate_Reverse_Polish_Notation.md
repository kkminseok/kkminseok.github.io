---
title: leetcode(리트코드)5월25일 challenge150-Evaluate Reverse Polish Notation(python)
date: 2021-05-25 04:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 25일 - Evaluate Reverse Polish Notation 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/evaluate-reverse-polish-notation/>  

![](/assets/img/sample/leetcode/150/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/150/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 25일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 연산자 후위표기법에 관한 문제입니다.
- 음수 나눗셈에 대한 처리를 잘 해주면 어렵지 않은 문제입니다.




-----  

## 5. code


**python**

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        #stack
        st =deque()
        
        for i in range(len(tokens)):
            token = tokens[i]
            #음수일 경우 digit()함수로 판단이 불가능 하므로 따로 분기문을 정해주었고, 값을 음수로 바꾸어 넣어줬습니다.
            if token[0] == "-" and len(token) >1 :
                minus = int(token[1:])
                minus = -minus
                st.append(minus)
            # 양수인 경우 stack에 넣습니다.
            elif token.isdigit() : 
                st.append(int(token))
            # 연산자인경우
            else : 
                second = st.pop()
                first = st.pop()
                if token == "+" : 
                    st.append(first+second)
                elif token == "-":
                    st.append(first-second)
                elif token == "*" : 
                    st.append(first*second)
                else : 
                    # 만약 둘 중하나가 음수인 경우는 절대값을 계산하여 -를 넣어줘야합니다.

                    # 6//-132 같은 경우 0이 리턴되어야하는데, 1인지 몫이 생겨버려서 이와같은 로직을 거쳐야합니다.
                    if (first < 0 and second > 0) or (first > 0 and second <0):
                        res = abs(first) // abs(second)
                        st.append(-res)
                    #둘 다 음수거나 양수인경우
                    else : 
                        st.append(first//second)
        #st[0]에는 최종값이 저장되어 있습니다.
        return st[0]
                    
```



-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/150/result.JPG)  

필요시 c++로 풀어드립니다.



