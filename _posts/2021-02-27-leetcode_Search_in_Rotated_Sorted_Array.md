---
title: leetcode(리트코드)33-Serach in Rotated Sorted Array
author: 강민석
date: 2021-02-27 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 33 - Search in Rotated Sorted Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/search-in-rotated-sorted-array/>  

![](/assets/img/sample/leetcode/33/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/33/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 문제 해석이 까다로웠습니다.
- 이미 정렬된 오름차순 벡터가 있다고 가정하고 어느 한 인덱스를 지점으로 뒤집어 버린것이 'nums' 벡터입니다. 그렇기에 [4,5,6,7,0,1,2]는 인덱스 [0,1,2,4,5,6,7]에서 인덱스 3을 기준으로 뒤집은 벡테입니다.
- 해당 벡터에서 target으로 주어진 값을 찾습니다. 있으면 해당index를 반환하고 없으면 -1을 반환합니다.

- 풀이과정은 맨끝 의 인덱스는 앞으로 갈수록 작아질 일만 남았고, 0번째 인덱스에서 어느 지점까지는 증가할 일만 남았으니 들어온 값이 0번째 인덱스를 기준으로 작으면 뒤에서부터 검사하고 아니면 순차접근 합니다. 최악의경우 n이겠지만..  


-----  

## 5. code

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.size()==1)
            return nums[0]==target ? 0 : -1;
        
        
        int start =0;
        int end = nums.size()-1;
        int max = nums[end];
        //끝에서부터 탐색(끝이 가까울 수록)
        if(target<=nums[end])
        {
            while(end>start && nums[end]>target)
            {
                cout<<end<<'\n';
                --end;
            }
            if(nums[end]==target)
                return end;
            else
                return -1;
        }
        else
        {
            while(start<end &&nums[start]<target)
            {
                ++start;
            }
            if(nums[start]==target)
                return start;
            else
                return -1;
        }
        
        return -1;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/33/result.JPG)  







