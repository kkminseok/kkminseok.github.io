---
title: leetcode(리트코드)-1047 Remove All Adjacent Duplicates In String(python)
author: 강민석
date: 2021-06-30 01:34:50 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1047 - Remove All Adjacent Duplicates In String  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/> 

![](/assets/img/sample/leetcode/1047/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1047/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 연속되는 문자는 제거합니다.
- 남는 문자열을 리턴합니다.
- 삭제하고 난 뒤도 검사를 해줘야하므로 스택을 사용하는 것이 편합니다.


-----  

## 5. code  

코드설명

**python**

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = ""
        st  = deque()
        for i in range(len(s)):
            if st :
                if st[-1] == s[i] : 
                    st.pop()
                else : 
                    st.append(s[i])
            else : 
                st.append(s[i])
        while st : 
            res+=st.popleft()
        return res
            
            
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1047/result.JPG)  

필요시 c++로 짜드리겠습니다.



