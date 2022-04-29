---
title: leetcode(리트코드)2월18일 challenge413-Arithmetic Slices
author: 강민석
date: 2021-02-18 17:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge18 - Arithmetic Slices 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/arithmetic-slices/>  

![](/assets/img/sample/leetcode/413/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/413/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월18일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 연속된 배열에서 앞과 뒤의 차이는 같아야합니다. 그 길이가 2 이상이 되면 만들 수 있는 조합의 개수를 만들어야합니다..
- 설명이 어려운데 1,2,3,4,5 들어오면 각각 1의 차이를 보이므로 나오는 값은 {1,2,3}, {2,3,4},{1,2,3,4,5},{1,2,3,4}... 이런 식으로 10개가됩니다.
- 저는 DP로 풀었습니다.

- 점화식은 DP[i] = 2 * DP[i-1] - (DP[i-2] + 1) 입니다.
- 설명하기 어려우므로 궁금하신 분은 물어보시면 알려드리겠습니다.




-----  

## 5. code

```c++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
        if(A.size()<3)
            return 0;
        int count=1;
        int temp = A[1]-A[0];
        int result = 0;
        int* DP=new int[A.size()+1];

        for(int i = 0;i<A.size()+1;++i)
            DP[i]=0;
        
        for(size_t i = 2;i<A.size();++i)
        {
            if(A[i]-A[i-1]==temp)
            {
                ++count;
            }
            else
            {
                count=1;
                temp = A[i]-A[i-1];
            }
            if(count>=2)
            {
                DP[i] = DP[i-1] + (DP[i-1] -DP[i-2] + 1);
            }
        }
        for(size_t i=0;i<A.size();)
        {
            while(DP[i+1]!=0 && i<A.size())
            {
                ++i;
            }
            result+=DP[i];
            ++i;
        }
        
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점
  
**시간(64%)**  
![](/assets/img/sample/leetcode/413/result.JPG) 

마지막에 DP를 도는 시간 때문에 오래걸린 것 같습니다.

**0ms(100%)코드**

```c++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
        int i,j,n=A.size(),c=0;
        if(n<3)
            return 0;
        for(i=0;i<n-2;i++)
        {
            for(j=i+2;j<n;j++)
            {
                if(A[j]-A[j-1]==A[j-1]-A[j-2])
                    c++;
                else
                    break;
            }
        }
        return c;
    }
};
```

아예 다 검사하는 코드입니다. 이렇게 간단해도 0ms가 나오다니..
브루트 포스로 풀어볼 걸 그랬습니다. 정해진 배열의 크기가 없어서 브루트 포스로 풀지 않았는데..



