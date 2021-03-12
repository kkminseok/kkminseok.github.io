---
title: leetcode(리트코드)9-Palindrome Number
author: 강민석
date: 2021-03-12 05:00:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 9 - Palindrome Number 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/palindrome-number/>  

![](/assets/img/sample/leetcode/9/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/9/input.JPG)  

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 간단한 문제입니다. 정수로 들어온 값이 Palindrome인지 판단합니다.
- string으로 바꿔서 풀었습니다.
- 풀고나서는 음수 값이 들어오면 Palindrome이 성립할 수 없기에 그냥 Brute Force로 풀었어도 되었을 것 같습니다.

-----  

## 5. code

**c++**

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        string temp = std::to_string(x);
        int left = 0;
        int right = temp.size()-1;
        while(left<=right)
        {
            if(temp[left++]!=temp[right--])
                return false;
        }
        return true;
    }
};
```

-----  

**python**

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        temp = str(x)
        left = 0
        right = len(temp)-1
        while(left<=right):
            if temp[left]!=temp[right]:
                return False
            left+=1
            right-=1
        
        return True
```

-----

## 6. 결과 및 후기, 개선점

**c++ 54% python 98%**  
![](/assets/img/sample/leetcode/9/result.JPG)  

 
 **c++ 100% 0ms code**

 ```c++
 class Solution {
public:
    bool isPalindrome(int x) {
        int temp = x;
        long rev, rem;
        rev = 0;
        
        if (x < 0)
            return false;
        
        while (temp != 0){
            rem = temp % 10;
            rev = rev * 10 + rem;
            temp = temp/10;
        }
        
        if (x == rev)
            return true;
        else
            return false;
    }
};
 ```
string으로 바꾸지않고 맨 뒤의 값과 맨 앞의 값을 비교하면서 푼 코드입니다.


