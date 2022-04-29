---
title: leetcode(리트코드)3월1일 challenge575-Distribute Candies
author: 강민석
date: 2021-03-01 01:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 01일 - Distribtue Candies 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/distribute-candies/>  

![](/assets/img/sample/leetcode/575/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/575/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
3월01일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 주어진 벡터 크기/2만큼 캔디를 먹을 수 있습니다.
- 하지만 그만큼 종류가 되지 않으면 캔디 종류를 리턴합니다.
- 오름차순이 아닐 수도 있다는 점을 고려하며 풀면 틀리지 않을 것 같습니다.

-----  

## 5. code

**c++**

```c++
class Solution {
public:
    int distributeCandies(vector<int>& candyType) {
        sort(candyType.begin(),candyType.end());
        int num = candyType.size()/2;
        int count = 1;
        for(size_t i =0;i<candyType.size()-1;++i)
        {
            if(candyType[i] != candyType[i+1])
                ++count;
        }
        return count > num ? num : count;
    }
};
```

**python**

```python
class Solution:
    def distributeCandies(self, candyType: List[int]) -> int:
            candyType.sort()
            num = int(len(candyType)/2)
            count = 1
            for i in range(len(candyType)-1):
                if candyType[i] != candyType[i+1]:
                    count+=1
            return num if count> num  else count
```

-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/575/result.JPG)  

**104ms 100% 코드**

```c++
class Solution {
public:
    int distributeCandies(vector<int>& candyType) 
    {
        int possible = candyType.size() / 2;
     
        bitset<200001> types(0);
        
        for(auto type : candyType)
        {
            types.set(type + 100000);
        }
        
        int typesNumber = types.count();
        
        return typesNumber < possible ? typesNumber : possible;
    }
};

```

bitset은 STL 입니다.
음수가 들어올 수 있으므로 넉넉하게 200001만큼 bitset을 만들고 음수를 대비하여 100000을 더합니다. 

참고 출처 : <https://www.crocus.co.kr/549>




