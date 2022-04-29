---
title: leetcode(리트코드)2월28일 challenge895-Maximum Frequency Stack
author: 강민석
date: 2021-03-01 00:00:00 +0800
categories: [leetcode,Hard]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February 28일 - Maximum Frequency Stack 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/maximum-frequency-stack/>  

![](/assets/img/sample/leetcode/895/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/895/input.JPG)  

Note:

- Calls to FreqStack.push(int x) will be such that 0 <= x <= 10^9.
- It is guaranteed that FreqStack.pop() won't be called if the stack has zero elements.
- The total number of FreqStack.push calls will not exceed 10000 in a single test case.
- The total number of FreqStack.pop calls will not exceed 10000 in a single test case.
- The total number of FreqStack.push and FreqStack.pop - calls will not exceed 150000 across all test cases.


-----  

## 3. 분류 및 난이도

Hard 난이도입니다.  
2월28일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- stack을 구현합니다. 다만 POP를 할 때 빈번한 순서대로 반환을 합니다. 만약 빈번한 정도가 같다면 stack의 top에 가까운 순서대로 반환합니다.

- 2월의 마지막 문제라 그런지 Hard문제를 줬습니다.

-----  

## 5. code

```c++
class FreqStack {
private:
    //map은 요소 찾기용입니다. 첫 번째 값은 요소, 두 번째 값은 빈번도 입니다. 4324 이 들어오면  map[4]는 2, map[3]은 1 map[2]는 1로 저장되어 있습니다.
    unordered_map<int,int> map;
    // st는 직접 관리할 스택입니다. 첫 번째 값은 push로 들어온 값. 두 번째 요소는 map에서 찾은 value값입니다.
    vector<pair<int,int>> st;
    //vec는 maxnum을 관리할 객체입니다. 54321로 들어오면 maxnum이 1이므로 vec[1]=5(개)입니다. 543215 로 5가 2개이므로 maxnum은 2가 되고, 들어오면 vec[1]= 5 vec[2] = 1 입니다.
    vector<int> vec;
    int maxnum=0;
    
public:
    FreqStack() {
        //0번째 요소를 만듭니다. 더미값.
        vec.push_back(0);
    }
    
    void push(int x) {
        //만약 들어온 값이 처음들어온 값이면
        if(map.find(x) == map.end())
        {
            //처음 들어온 값이므로 1만큼 카운트값을 세주고
            map[x]=1;
            //아예 처음들어온 값이면
            if(maxnum==0)
            {
                maxnum=1;
                vec.push_back(1);
            }
            else
            {
                //아니면 이미 vec[1]은 존재하므로
                vec[1]++;
            }
        }
        //이미 stack에 값이 존재하면
        else
        {
            //카운트 값을 증가시킵니다.
            map[x]++;
            if(map[x]>maxnum)
            {
                maxnum=map[x];
                //만약 vec요소에 접근할 수 없을만큼 큰 값이면
                if(vec.size()<=maxnum)
                    vec.push_back(1);
                else
                {
                    vec[maxnum]++;
                }
            }
            //vec요소에 접근할 수 있습니다.
            else
                vec[map[x]]++;
        }
        st.push_back(make_pair(x,map[x]));
    }
    
    int pop() {
        int size = st.size()-1;
        int temp = st[size].first;
        if(map[temp] == maxnum)
        {
            map[temp]--;
            st.pop_back();
            vec[maxnum]--;
            //최대값을 갱신합니다.
            if(vec[maxnum]==0)
            {
                --maxnum;
            }
            return temp;
        }
        else
        {
            for(;size>=0;--size)
            {
                //최대값이랑 같은 count면 지워버립니다.
                if(st[size].second == maxnum)
                {
                    map[st[size].first]--;
                    int returnnum = st[size].first;
                    st.erase(st.begin() + size);
                    vec[maxnum]--;
                    if(vec[maxnum]==0)
                    {
                        --maxnum;
                    }
                    return returnnum;
                }
            }
        }

        return 1;
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 */
```

-----

## 6. 결과 및 후기, 개선점

2시간 걸렸습니다..

그래도 공간 복잡도가 상위권으로 나왔군요..

![](/assets/img/sample/leetcode/895/result.JPG)  

**136ms 100% 코드**

```c++
//입출력을 빠르게 하는 코드인것 같습니다.
#pragma GCC optimize("Ofast")
static auto _ = [] () {ios_base::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);return 0;}();

class FreqStack
{
public:
    unordered_map<int, int> freqMap;
    unordered_map<int, vector<int>> stackMap;
    int maxFreq = 0;
    
    FreqStack()
    {}
    
    void push(int x)
    {
        int freq = ++freqMap[x];
        //이게 되네.
        stackMap[freq].push_back(x);
        maxFreq = max(maxFreq, freq);
    }
    
    int pop()
    {
        auto& stack = stackMap[maxFreq];

        int x = stack.back();
        stack.pop_back();
        
        --freqMap[x];
        if (stack.size() == 0)
            --maxFreq;
        
        return x;
    }
};

```

이해하기 간단한 코드입니다..

map을 저렇게 활용할 수 있다는 건 몰랐습니다.



