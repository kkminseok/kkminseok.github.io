---
title: leetcode(리트코드)338-Counting Bits
author: 강민석
date: 2021-03-21 03:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Facebook]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 338 - Counting Bits 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/counting-bits/>  

![](/assets/img/sample/leetcode/338/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/338/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
<https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU에서> 추천한 문제입니다.


-----  

## 4. 문제 해석

- 주어진 트리에서 가장 긴 경로를 가진 길이를 구하세요.
- 트리의 root를 지날 수도 안지날 수도 있습니다.
- 왼쪽자식 + 오른쪽 자식의 합을 매 트리의 노드마다 구해줘서 갱신했습니다.


-----  

## 5. code


**c++**

```c++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> DP(num+1);
        DP[0] = 0 ;
        if(num ==0)
            return DP;
        DP[1] = 1;
        int Two_exp = 1;
        for(int i = 1;i<=num;++i)
        {
            if(i == pow(2,Two_exp))
            {
                DP[i] = 1;
                ++Two_exp;
            }
            else
            {
                int pownum = pow(2,Two_exp-1);
                DP[i] = DP[pownum] + DP[i-pownum];
            }
        }
        
        return DP;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 94% python ??%** 
![](/assets/img/sample/leetcode/338/result.JPG)  




**c++ 0ms 100% code**

```c++
class Solution {
public:
    int ans = 0;
    int util(TreeNode *root){
        if(!root) return 0;
        int left = util(root->left);
        int right = util(root->right);
        ans = max(left + right, ans);
        return max(left, right) + 1;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        util(root);
        return ans;
    }
};
```
제 로직에서 쓸데없는 재귀를 없애버린 코드입니다.



