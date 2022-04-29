---
title: leetcode(리트코드)941-Valid Mountain Array
author: 강민석
date: 2021-02-11 14:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Valid Mountain Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/number-of-1-bits/>  

![](/assets/img/sample/leetcode/191/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/191/input.JPG)

-----  

## 3. 분류 및 난이도

Eazy 난이도입니다.  

-----  

## 4. 문제 해석

- 배열을 돌면서 오름차순, 내림차순이 한 번 있으면 true를 반환합니다. 값이 같으면 안됩니다.  

-----  

## 5. code

```c++
class Solution {
public:
    bool validMountainArray(vector<int>& arr) {
        bool downhill =false;
        for(size_t i=0;i<arr.size()-1;++i)
        {
            if(arr[i]==arr[i+1])
                return false;
            //내리막길
            if(arr[i]>arr[i+1])
                downhill=true;
            if(downhill && i==0)
                return false;
            //오르막길
            if(downhill && arr[i] <=arr[i+1])
                return false;
            
        }
        return downhill;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/941/result.JPG)  

**0ms인 사람의 java코드**  
```java
class Solution {
    public boolean validMountainArray(int[] A) {
        int N = A.length;
        int i = 0;

        // walk up
        while (i+1 < N && A[i] < A[i+1])
            i++;

        // peak can't be first or last
        if (i == 0 || i == N-1)
            return false;

        // walk down
        while (i+1 < N && A[i] > A[i+1])
            i++;

        return i == N-1;
    }
}

```

코드 해석을 하면, 올라가는 길인 경우 인덱스를 계속 더해줍니다.  
그 이후 i==0(오르막길이 없었음) or i==N-1(끝의 인덱스까지 쭉 오르막길인 경우) return false를 해줍니다.  
그 이후는 내리막길이므로 ++i를 해주고 i가 N-1(마지막 인덱스)인 경우 true를 반환합니다. 두 개의 산맥이 있을 경우를 제외한 것 같습니다.  


