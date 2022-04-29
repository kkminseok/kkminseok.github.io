---
title: leetcode(리트코드)78-Subsets
author: 강민석
date: 2021-03-12 00:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 78 - Symmetric Tree 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/subsets/>  

![](/assets/img/sample/leetcode/78/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/78/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 중복되지 않게 수를 나열하고 그 수를 담은 벡터를 리턴합니다.

-----  

## 5. code

**python**

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        
        self.result = []
        self.solution(0,nums,[])
        return self.result
    
    def solution(self, depth : int, nums : List[int], temp : List[int]):
        if depth == len(nums):
            self.result.append(temp)
            return 
        temp.append(nums[depth])
        self.solution(depth+1,nums,temp[:])
        temp.pop()
        self.solution(depth+1,nums,temp[:])        
        
```

-----  

**c++**

```c++
class Solution {
public:
    void solution(vector<vector<int>>& vec,int depth,vector<int>& nums,vector<int> temp)
    {
        if(depth == nums.size())
        {
            vec.push_back(temp);
            return;
        }
        temp.push_back(nums[depth]);
        solution(vec,depth+1,nums,temp);
        temp.pop_back();
        solution(vec,depth+1,nums,temp);
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;
        vector<int> temp;
        solution(result,0,nums,temp);
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 64% python 94%**  
![](/assets/img/sample/leetcode/78/result.JPG)  

 
c++100% 코드는 for문 3개로 brute force하게 풀었는데.. 제가 더 느리다니..?
