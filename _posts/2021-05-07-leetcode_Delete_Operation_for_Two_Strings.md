---
title: leetcode(리트코드)5월7일 challenge583-Delete Operation for Two Strings
date: 2021-05-07 12:12:56 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode May 7일 - Delete Operation for Two Strings 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/delete-operation-for-two-strings/>  

![](/assets/img/sample/leetcode/583/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/583/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
5월 7일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 두개의 string이 주어집니다.
- word1에서 word2로 넘어가기 위해 문자를 삭제, 삽입할 수 있는데 최소한의 연산을 사용해야합니다.
- 최소한의 연산을 사용했을 때 그 갯수를 리턴합니다.





-----  

## 5. code

LCS를 통해 공통 부분 문자열을 찾아 해당 문자열로 만드는 과정 (word1 - DP[m][n]<- LCS)  + word2 -DP[m][n] <-LCS LCS를 word2로 만드는 과정을 코드로 담았습니다.


**c++**

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        //LCS
        int size1 = word1.size();
        int size2 = word2.size();
        int** DP = new int*[size1+1];
        for(int i = 0 ; i<=size1;++i)
            DP[i] = new int[size2+1];
        
        for(int i = 0 ; i <=size1;++i)
            DP[i][0] = 0;
        for(int j = 0 ; j <=size2;++j)
            DP[0][j] = 0;
        
        for(int i = 1 ; i <=size1;++i){
            for(int j = 1 ; j <=size2;++j){
                    if(word1[i-1] == word2[j-1])
                        DP[i][j] = DP[i-1][j-1] + 1;
                    else
                        DP[i][j] = max(DP[i-1][j],DP[i][j-1]);
                }
            }
            return size1 + size2 - (DP[size1][size2] *2);   
    }
};
```

-----

## 6. 결과 및 후기, 개선점

![](/assets/img/sample/leetcode/583/result.JPG)  




