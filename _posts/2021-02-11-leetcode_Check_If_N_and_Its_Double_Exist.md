---
title: leetcode(리트코드)1346-Check If N and Its Double Exist
author: 강민석
date: 2021-02-11 13:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Remove Duplicates from Sorted Array 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-n-and-its-double-exist/>  

![](/assets/img/sample/leetcode/1346/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1346/input.JPG)

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  
eazy난이도의 문제입니다.  

-----  

## 4. 문제 해석

- 배열을 돌면서 자기 자신을 2로나눈 값이 있는지 찾는 문제입니다. 예를 들어서 10을 2로나눈 5라는 값을 가진 인덱스가 있는 지 찾고 있으면 true, 없으면 false를 리턴합니다.

-----  

## 5. code

```c++
class Solution {
public:
    //음수에도 값의 최대인 1천을 더해 인덱스에 바로 접근할 수 있게 합니다.
    // 예를 들어서 -8 값을 가진 인덱스라면 -8 + 1000한 992의 값을 증가시킵니다.
    int check[2002]={0,};
    bool checkIfExist(vector<int>& arr) {
        //arr를 돌면서 값을 더해줍니다.
        for(size_t i=0;i<arr.size(); ++i)
        {
            check[arr[i]+1000]++;
        }
        for(size_t i=0;i<arr.size();++i)
        {
            // 배열의 요소가 minus인 경우
            bool minus=false;
            if(arr[i]<0)
                minus=true;    
            int nums = abs(arr[i]);
            //0인 경우는 조심해야합니다. 0이 두 번 들어왔으면 true를 리턴해야하고 1번만 들어왔으면 넘어가야합니다.
            if(nums==0)
            {
                if(check[1000]>1)
                    return true;
                else
                    continue;
            }
            //홀수는 제외시킵니다.
            if(nums%2==1)
            {
                continue;
            }
            int result = nums/2;
            if(minus)
                result=-result;
            // 값이 있는 지 찾습니다.  
            if(check[result+1000])
            {
                return true;
            }
        }
        return false;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
![](/assets/img/sample/leetcode/1346/result.JPG)  

**0ms인 사람의 코드**  
```c++
class Solution {
public:
    bool checkIfExist(vector<int>& arr) {
        //map을 선언합니다. unordered는 그냥 map(레드블랙 트리로 구성되어 정렬되어 들어감)과 달리 해시 테이블 기반으로 정렬되어 값이 들어가지 않습니다.
        unordered_map<int, int> v;
        
        for(int i=0;i<arr.size();i++){
            //만약 짝수이고 결과를 찾았다면.
            if(arr[i]%2==0 && v.find((arr[i]/2))!=v.end()){
                cout<<"1";
                return true;
            }
            //반대로 자기 자신의 * 2한 값을 찾았다면
            if(v.find((arr[i]*2))!=v.end()){
                cout<<"2";
                return true;
            }
            //해시에 값을 넣습니다.
            if(v.find(arr[i])==v.end()){
                //not present
                v[arr[i]] = 1;
            }
        }
        return false;
    }
};
```

공간복잡도 굳이 개선할 점 없음.  
