---
title: leetcode(리트코드)448-Find All Numbers Disappeared in an Array
author: 강민석
date: 2021-02-13 14:30:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Find All Numbers Disappeared in an Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/>  

![](/assets/img/sample/leetcode/448/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/448/input.JPG)

-----  

## 3. 분류 및 난이도

Eazy 난이도 문제입니다.  

-----  

## 4. 문제 해석

- 배열의 크기만큼 수열이 지속되어야합니다. 예를 들어서 배열의 크기가 8이면 1~8까지의 수가 있어야합니다. 만약 없으면 없는 숫자를 배열에 넣어 리턴합니다.

- 처음에는 추가공간을 쓰지 않고 작성하기 위해 노력을 많이했습니다. 하나의 for문안에서 처리하려고 하고, vetor.erase(), vector.push_back()을 이용해서 풀려고 하였으나, 버퍼 오버플로가 발생하여 포기..
    + 추가 공간을 사용하지 않고 푼 사람이 있나 Discussion에 들어가서 확인해보았습니다. 1page에서 c++로 푼 사람 중에는 그런 사람이 없었습니다.
- 추가공간을 사용하기로 하였지만, 하나의 for문안에서 처리하려고 또 노력했습니다만 포기했습니다. (이 문제에 거의 1시간을 써버렸기에)

- 최후의 수단으로 생각했던 방문처리 배열을 둬서 풀기로 했습니다. 풀렸지만 찝찝했습니다.
    + 문제에서 **배열의 크기 제한**을 두지 않아서 1억개가 들어올 지 2억개가 들어올 지 가늠이 안되서 위험한 시도라고 생각했습니다.



-----  

## 5. code

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> result ;
        int size = nums.size();
        bool* check = new bool[nums.size()+1];
        memset(check,false,sizeof(bool) * (nums.size()+1));
        for(int i = 0;i<size;++i)
        {
            check[nums[i]]=true;
        }
        for(int i = 1;i<=size;++i)
        {
            if(check[i]==false)
                result.push_back(i);
        }
        
        
        return result;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/448/result.JPG)  


개선사항없음






