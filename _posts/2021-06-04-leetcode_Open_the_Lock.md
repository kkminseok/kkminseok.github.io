---
title: leetcode(리트코드)6월04일 challenge752-Open the Lock(python)
date: 2021-06-04 05:01:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode June 04일 - Open the Lock 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/open-the-lock/>  

![](/assets/img/sample/leetcode/752/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/752/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
6월 04일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 초기값이 '0000'인 슬롯이 있습니다. 해당 슬롯을 위아래(0인 경우 9가 아래 9인 경우 0이 위)로 움직여서 target을 만들어야합니다.
- deadends는 슬롯이 해당 숫자에 도달하면 더 이상 작동하지 않기에 해당 값에 도달하지 않도록 해야합니다.
- 만들 수 없으면 -1을 리턴하고 만들 수 있다면 최소값을 리턴하세요.




-----  

## 5. code


**python**

```python
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        #BFS
        deadset = set(deadends)
        #방문처리
        v = set('0000')
        dq = deque([('0000',0)])
        
        while dq : 
            slot,counting = dq.popleft()
            if slot == target : 
                return counting
            elif slot in deadset : 
                continue
            for i in range(4) : 
                digit = int(slot[i])
                for move in [-1,1]:
                    #0 - 1 %10은 나머지가 9입니다. 10을 1번 빼주고 (-10)에서 -1이 되려면 나머지 9를 더해야하기 때문입니다.
                    newdigit = (digit + move)%10
                    newslot = slot[:i] + str(newdigit) + slot[i+1:]
                    #방문처리
                    if newslot not in v:
                        v.add(newslot)
                        dq.append((newslot,counting+1))
        return -1     
```


-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/752/result.JPG)  

필요시 c++로 풀어드립니다.



