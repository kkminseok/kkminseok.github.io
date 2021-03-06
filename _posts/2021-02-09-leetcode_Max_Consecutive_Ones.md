---
title: leetcode(리트코드)485-Max Consecutive Ones
author: 강민석
date: 2021-02-09 12:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Max Consecutive Ones 문제입니다.**

## 1. 문제
<https://leetcode.com/explore/learn/card/fun-with-arrays/521/introduction/3238/>  

![](/assets/img/sample/leetcode/3238/Problem.JPG)

-----  

## 2. Input , Output

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  
eazy난이도의 문제입니다.  
어렵지 않고, leetcode가 어떤 시스템인지 인지하기 위해 풀었습니다.  



-----  

## 4. 문제 해석

- 해석은 배열 속에 있는 '1'의 연속적인 수열의 최대크기를 찾아 리턴하는 문제입니다. 즉 '0'이 하나 들어오면 0을 리턴 '1'이 하나 들어오면 1를 리턴해야합니다.  




-----  

## 5. code

```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int countz = 0;
        int result =0;
        if(nums.size()==1 &&nums[0]==0)
            return 0;
        for(size_t i=0;i<nums.size();++i)
        {
            if(nums[i]==0)
                countz=0;
            else if(nums[i]==1)
            {
                countz++;
            }
            if(result<countz)
                result =countz;

        }
        return result;
    }
 
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**
![](/assets/img/sample/leetcode/3238/result1.JPG)  

**메모리**
![](/assets/img/sample/leetcode/3238/result2.JPG)  

**다시 제출했을 때**  
![](/assets/img/sample/leetcode/3238/result3.JPG)  


leetcode의 첫 문제인데 leetcode가 마음에 드는 점은 틀렸을 때 테스트코드를 공개해주고, 시간과 메모리사용량 순위를 보여줘서 더 개선하고 싶은 자극(?)이 든다.  

근데 똑같은 코드를 제출할 때마다 시간이 다르게 측정되는 것 같은데 오해일수도 있으므로 다른 문제들을 풀면서 알아봐야겠다.  


**제일 많은 사람이 본 코드**

직접 실행해보니 나와 시간이 똑같고 메모리는 오히려 더 많이 사용했다.. ?? simple code라 이해하기 쉽게 작성한 코드라 그런가?? 잘 모르겠다.  

```c++
int findMaxConsecutiveOnes(vector<int>& nums) {
    int max_cnt = 0, cnt = 0;
    //auto를 지원하는 지 몰랐다. 사실 알아도 안썼음.
    for (auto n : nums) {
        // 나도 max를 사용하려 했지만, leetcode에 익숙하지 않아서 라이브러리를 불러올때마다 런타임 에러가 발생해서 그냥 안썼다.. 직접해보니 그냥 라이브러리가 기본적으로 적용되어있다..!
        if (n == 1) max_cnt = max(++cnt, max_cnt);
        else cnt = 0;
    }
    return max_cnt;
}
```