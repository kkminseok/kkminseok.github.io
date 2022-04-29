---
title: leetcode(리트코드)977-Squares of a Sorted Array
author: 강민석
date: 2021-02-09 14:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Squares of a Sorted Array 문제입니다.**

## 1. 문제
<https://leetcode.com/explore/learn/card/fun-with-arrays/521/introduction/3240/>  

![](/assets/img/sample/leetcode/3240/Problem.JPG)  

-----  

## 2. Input , Output

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  
eazy난이도의 문제입니다.  
어렵지 않고, leetcode가 어떤 시스템인지 인지하기 위해 풀었습니다.  



-----  

## 4. 문제 해석

- 직관적입니다. 배열에 들어온 값을 한 변의 길이라고 생각하고 사각형의 넓이를 구한 뒤, 정렬해서 리턴하면 됩니다. 
- 들어온 값은 이미 오름차순으로 정렬되어 있습니다.  




-----  

## 5. code

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for(size_t i=0;i<nums.size();++i)
            nums[i]*= nums[i];
        sort(nums.begin(),nums.end());
        return nums;
    }
};
```
-----

## 6. 결과 및 후기, 개선점
  
제가 제출한 코드의 시간과 메모리를 첨부하지 않기로 하였습니다.  
이유는 매 번 제출할 때마다 결과가 다르게 나오기 때문입니다. 

![](/assets/img/sample/leetcode/3240/result.JPG)  


**시간효율성이 좋은 코드**


```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n = nums.size();
        //nums의 사이즈만큼의 크기를 가진 res벡터 
        vector<int> res(n);
        //Two pointer
        int left = 0;
        int right = n-1;
        int i = n-1;
        //양쪽에서 비교하면서 절대값이 큰 값을 배열의 맨 끝부터 인덱스 0까지 값을 집어 넣습니다.
        while(left<=right)
        {
            if(abs(nums[left]) > abs(nums[right]))
            {
                res[i] = nums[left]*nums[left];
                left++;
            }
            else
            {
                res[i] = nums[right]*nums[right];
                right--;
            }
            i--;
        }
        return res;
    }
};
```
  
**공간 효율성이 좋은 코드** 

위의 코드와 같아서 따로 적진 않겠습니다.  
