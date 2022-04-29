---
title: leetcode(리트코드)2월17일 challenge11-Container With Most Water
author: 강민석
date: 2021-02-18 12:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge17 - Container With Most Water 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/container-with-most-water/>  

![](/assets/img/sample/leetcode/11/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/11/input.JPG)  

**Constraints:**

- n == height.length
- 2 <= n <= 3 * 104
- 0 <= height[i] <= 3 * 104

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월17일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 막대 두개를 잡아서 그 막대안의 사각형의 최댓값을 구해 리턴합니다.
- 당연하겠지만 브루트 포스로 이중 포문으로 접근하면 시간초과뜹니다.(제가해봄..)
- 그러면 양쪽에서 값을 비교하면서 접근하는 수밖에 없다고 생각했습니다.

-----  

## 5. code

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int result = 0;
        //왼쪽 인덱스
        int left = 0;
        //오른쪽 인덱스
        int right = height.size()-1;
        while(left<=right)
        {
            //둘 중 작은 값을 기준으로 해야하므로
            int line = min(height[left],height[right]);
            //넓이 계산
            int area = line * (right-left);
            //큰 값이면 갱신해줘야하므로
            result = max(area,result);
            // 작은 값을 옮겨야합니다. 다음은 큰값이 들어올 수도 있기 때문이죠. 
            if(height[left]<height[right])
                ++left;
            else
                --right;
        }
        
        return result;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**시간(95%)**

![](/assets/img/sample/leetcode/11/result.JPG)  


