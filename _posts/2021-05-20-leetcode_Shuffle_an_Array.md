---
title: leetcode(리트코드)-384 Shuffle an Array(python)
author: 강민석
date: 2021-05-20 03:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Interview]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 384 - Shuffle an Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/shuffle-an-array/> 

![](/assets/img/sample/leetcode/384/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/384/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
Top Interview 문제입니다.


-----  

## 4. 문제 해석

- 배열에 들어오는데, reset()함수는 처음 들어온 배열을 / shuffle()함수는 기존 배열의 요소값들을 랜덤으로 바꿔서 리턴하는 함수를 작성합니다.


-----  

## 5. code  

코드설명


**python**

```python
class Solution:
    
    def __init__(self, nums: List[int]):
        self.nums = nums

    def reset(self) -> List[int]:
        return self.nums
        """
        Resets the array to its original configuration and return it.
        """
        

    def shuffle(self) -> List[int]:
        ans = self.nums[:]                     # copy list
        for i in range(len(ans)-1, 0, -1):     # start from end
            # i+1 까지의 인덱스 중 하나를 골라 j에 넣습니다.
            j = random.randrange(0, i+1)    # generate random index 
            ans[i], ans[j] = ans[j], ans[i]    # swap
        return ans
        """
        Returns a random shuffling of the array.
        """
        


# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.reset()
# param_2 = obj.shuffle()
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/384/result.JPG)  

필요시 c++로 짜드리겠습니다.



