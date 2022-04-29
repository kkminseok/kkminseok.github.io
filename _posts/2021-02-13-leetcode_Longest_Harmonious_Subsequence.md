---
title: leetcode(리트코드)2월4일 594-Longest Harmonious Subsequence
author: 강민석
date: 2021-02-13 16:30:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge04 - Logest Harmonious Subsequence 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-harmonious-subsequence/>  

![](/assets/img/sample/leetcode/594/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/594/input.JPG)  


-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  
2월04일자 챌린지 문제입니다.   

-----  

## 4. 문제 해석

- 최대값과 최소값이 1이 차이가 나는 가장 긴 부분 배열을 찾고 그 길이를 리턴합니다.  


-----  

## 5. code

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        //결과
        int result =0;
        //같은 숫자를 셉니다.
        int countnumf = 1;
        //1 을 더한 숫자를 세줍니다.
        int countnums = 0;
        //위치
        int spot = nums[0];
        // 1 더한 숫자를 세주는 루프문을 한 번이라도 도는지 확인합니다.
        bool check = false;
        for(size_t i=0;i<nums.size();++i)
        {
            cout<<"i : "<<i<<'\n';
            spot = nums[i];
            //같은 값을 세줍니다.
            while(i+1<nums.size() && spot == nums[i+1])
            {
                ++countnumf;
                ++i;
            }
            spot = nums[i]+1;
            //1 더 큰 값을 세줍니다.
            while(i+1<nums.size() && spot ==nums[i+1])
            {
                check = true;
                ++countnums;
                ++i;
            }
            cout<<countnumf<<" "<<countnums<<'\n';
            if(countnums!=0)
                result = max(countnumf + countnums ,result);
            countnumf = countnums == 0 ? 1 : countnums;
            countnums = 0;
            //만약 1 더큰값을 세지 않은 경우 --i를 해줍니다. 예를 들어서 1112223라는게 있으면  2에서 3으로 넘어가버리기 때문에 --i를 통해 2로 유지해줘야합니다.
            if(check)
            {
                --i;
                check=false;
            }
            
        }
        
        return result;
    }

};
```

-----

## 6. 결과 및 후기, 개선점
  

![](/assets/img/sample/leetcode/594/result.JPG) 

시간이 매우 느리게 나왔습니다.

**이해하기 쉽고 빠른 코드(36ms)**

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        //오름차순 정렬
        sort(nums.begin(), nums.end());
        
        int max_n = 1;
        int max_n_1 = 0;
        int res = 0;

        for (int i = 1; i < nums.size(); ++i)
        {
            //만약 같은 숫자면
            if (nums[i] == nums[i - 1])
            {
                ++max_n;
            }
            else
            {
                //같은 숫자가 아닌데 밑의 if문을 한번이라도 돌았으면
                if (max_n_1 > 0)
                {
                    //결과를 넣어줍니다.
                    res = max(res, max_n + max_n_1);
                }
                //i+1 배열이 i 인덱스의 값보다 1클경우
                if (nums[i] - nums[i - 1] == 1)
                {
                    // 맨 위의 if문에서 세준 값을 넣어줍니다.
                    max_n_1 = max_n;
                }
                //그런 경우가 없을 경우 (차이가 많이남)
                else
                {
                    //아예 안돌았다는 표시
                    max_n_1 = 0;
                }
                
                max_n = 1;
            }
        }
        
        // 최대값을 구하는 if문을 한번이라도 돌았으면
        if (max_n_1 > 0)
        {
            res = max(res, max_n + max_n_1);
        }
        
        return res;
    }
};

```