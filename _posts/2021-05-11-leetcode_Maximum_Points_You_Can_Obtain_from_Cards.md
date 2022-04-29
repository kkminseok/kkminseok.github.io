---
title: leetcode(리트코드)5월11일 challenge1423-Maximum Points You Can Obtain from Cards
date: 2021-05-07 12:12:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 11일 - Maximum Points You Can Obtain from Cards 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/>  

![](/assets/img/sample/leetcode/1423/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1423/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 11일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- cardPoints라는 벡터가 들어옵니다. 맨 뒤와 맨 앞 중 골라 k만큼 뽑았을 때 그 포인트의 합계가가 가장 클 때의 값을 리턴합니다.





-----  

## 5. code

1,2,3,4,5,6,1 기준으로 k가 3일때 맨앞에서 부터 더하면 1,2,3,x,x,x,x 입니다.
3을 제외하고 뒤에있는 1을 뽑은 경우 1,2,x,x,x,x,1 인 경우가 됩니다.  
같은 방식으로 2를 제외하고 뒤에 있는 6을 뽑으면 1,x,x,x,x,6,1인 경우가 되고 마찬가지로  
x,x,x,x,5,6,1 을 뽑은 이 경우가 최선의 답이므로 리턴합니다.

우선순위 큐를 이용해서 값이 큰 것을 자동으로 큐 앞에 넣어 해당 값을 리턴하도록 작성하였습니다.


**c++**

```c++
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int ans = 0 ;
        for(int i = 0 ; i < k; ++i){
            ans += cardPoints[i];
        }
        if(k == cardPoints.size())
            return ans;
        
        int pick = 1;
        int right = cardPoints.size()-1;
        priority_queue<int> pq;
        pq.push(ans);
        while(pick <= k){
            ans -= cardPoints[k - pick] ;
            ans += cardPoints[right--];
            ++pick;
            pq.push(ans);
        }
        
        return pq.top();
        
    }
};
```

-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/1423/result.JPG)  




