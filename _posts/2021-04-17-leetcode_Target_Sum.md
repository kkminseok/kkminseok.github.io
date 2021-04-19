---
title: leetcode(리트코드)494-Target Sum
author: 강민석
date: 2021-04-17 05:34:50 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 494 - Target Sum 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/target-sum/> 

![](/assets/img/sample/leetcode/494/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/494/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 일반 벡터와 target이 주어집니다.
- 벡터의 요소들을 덧셈과 뺄셈을이용하여 target을 만들 수 있는 경우의 수를 리턴합니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    void DFS(vector<int>& nums,int target,int curr,int index,int& result){
        if(index == nums.size()){
            if(curr == target)
                ++result;    
            return ;
        }
        DFS(nums,target,curr-nums[index],index+1,result);
        DFS(nums,target,curr + nums[index],index+1,result);
    }
    
    int findTargetSumWays(vector<int>& nums, int target) {
        //DFS
        int result = 0 ;
        DFS(nums,target,0,0,result);
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점


**c++ 17% 1144ms**

![](/assets/img/sample/leetcode/494/result.JPG)  


**개선한 코드 4ms c++**


```c++
class Solution {
public:
    typedef  long long ll;
    int findTargetSumWays(vector<int>& a, int target) {
        ll i,j,sum=0,n=a.size();
        for(i=0;i<n;i++){
            //요소를 일단 다 더한다.
            sum+=a[i];
        }
        //target을 만들 수 없는 경우
        if(target>sum)
            return 0;
        
        //왠지 모르게 안되는 겅우?
        sum = (sum+target);
        if(sum%2!=0)
            return 0;
        

        //knap dp방법.
        ll k = (sum)/2;
        ll dp[n+1][k+1];
        memset(dp, 0, sizeof(dp));
        for(i=0;i<=n;i++){
            dp[i][0]=1;
        }
        for(i=1;i<=n;i++){
            for(j=0;j<=k;j++){
                if(a[i-1]<=j){
                    dp[i][j]=dp[i-1][j] + dp[i-1][j-a[i-1]];
                }else{
                    dp[i][j]=dp[i-1][j];
                }
            }
        }
        return dp[n][k]; 
    }
};
```


