---
title: leetcode(리트코드)-1247 Minimum Swaps to Make Strings Equal(PYTHON)
author: 강민석
date: 2021-08-28 02:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 1247 - Minimum Swaps to Make Strings Equal  문제입니다.**

## 1. 문제
<https://leetcode.com/problems/minimum-swaps-to-make-strings-equal/> 

![](/assets/img/sample/leetcode/1247/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1247/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  


-----  

## 4. 문제 해석

- 'x'와 'y'로 이루어진 두개의 문자열이 들어옵니다.
- 두 문자를 스왑할 수 있을 때 가장 적게 스왑하여 두 문자를 같게 만들어야합니다.
- 그 수를 리턴하세요.





-----  

## 5. code  

코드설명

- s1이 'xxxxxyyy'라고 하고, s2가 'yyyyyxxx'라고 하면 s1기준으로 x가 다른부분은 5개 y가 다른 부분은 3개입니다.
- 이 총합이 짝수가 아니면 -1을 리턴해야합니다.  만약 홀수라면 둘 중 하나의 갯수가 홀수라는 것인데, 그러면 두 글자를 절대 맞춰줄 수 없기 때문입니다.
- x기준으로 swap을 해줍니다. 위의 예제에서 'xxxxx'인데 일단 홀수개 이므로 s1의 'xxxx'와 s2의 'yyyy'만 바꿔줍니다.(xcount // 2 코드)
- 그 결과 'xyxyxyyy'가 되고, s2는 'xyxyyxxx'가 됩니다.
- 마찬가지로 y를 기준으로 바꿔줍니다. s1에서 'yyy'입니다. (ycount // 2 코드)
- 그 결과 'xyxyxyxy'가 되고, s2는 'xyxyyxxy'가 됩니다. 
- 다른부분은 s1의 'xy'와 s2의 'yx'가 되는데, 이는 특별한 경우로 swap을 2번 진행해줘야합니다. 이 경우는 x의 기준으로든 y의 기준으로든 홀수인 경우에만 나타납니다.
(xcount%2 == 1 : 코드 x기준으로)


**python**

```python
class Solution:
    def minimumSwap(self, s1: str, s2: str) -> int:
        xcount = 0 
        ycount = 0 
        res = 0 
        for i in range(len(s1)) : 
            if s1[i] != s2[i] : 
                if s1[i] == 'x' : 
                    xcount += 1
                else :
                    ycount += 1
        # 두문자의 다른부분이 홀수인 경우 -1
        if (xcount + ycount) % 2 == 1: 
            return -1
        else : 
            #맞춘 상태에서 시작
            res = (xcount // 2) + (ycount // 2) 
            if xcount %2 == 1 : 
                res += 2
        return res
        
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/1247/result.JPG)  



필요시 c++로 짜드립니다.

설명이 필요하다면 댓글을 달아주세요.


