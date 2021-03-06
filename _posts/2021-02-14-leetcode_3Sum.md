---
title: leetcode(리트코드)15-3Sum
author: 강민석
date: 2021-02-14 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 15 - 3Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/3sum/>  

![](/assets/img/sample/leetcode/15/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/15/input.JPG)

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 첫 번째 문제입니다.  


-----  

## 4. 문제 해석

- 세 숫자의 합이 0이되는 세 숫자를 찾고 result vector에 넣어야합니다. 중복이 있으면 안됩니다.  
- 중복 처리를 해주느라 시간을 많이써서 솔루션을 보고 이해했습니다.

- 정렬을 해준 뒤 앞의 원소를 c를 두고 앞 뒤에서 합을 이용해 찾습니다. 



-----  

## 5. code

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        
        for(size_t i=0;i<nums.size();++i)
        {
            int target = -nums[i];
            int front = i+1;
            int back = nums.size()-1;
            
            //모두 양수로 들어올 경우
            if(target <0)
                break;
            else
            {
                while(front<back)
                {
                    int sum = nums[front] + nums[back];
                    if(target>sum)
                        ++front;
                    else if(target<sum)
                        --back;
                    //같은 경우
                    else
                    {
                        vector<int> temp(3,0);
                        temp[0] = nums[i];
                        temp[1] = nums[front];
                        temp[2] = nums[back];
                        result.push_back(temp);
                    
                        //[-2,0,0,2,2]
                        //-2 target index(1) 0 = front index(4) 2 = back인 경우 -2,0,2 추가되는데 밑의 처리를 해주지 않으면 그 다음 인덱스인 (2) 0 = front (3) 2 = back이 또 다시 중복으로 들어가게됨.
                        while(front<back && nums[front] == temp[1]) ++front;
                        while(front<back && nums[back] == temp[2])--back;
                        
                    }
                }
                while(i+1<nums.size() && nums[i+1] == nums[i])
                    ++i;
            }
        }    
        
        
        
        return result;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
솔루션을 보았으니 사진을 넣지 않겠습니다.   
