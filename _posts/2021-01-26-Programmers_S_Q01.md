---
title: Programmers_Stack/Queue01 - 주식 가격
author: 강민석
date: 2021-01-26 15:01:00 +0800
categories: [Algorithm,Stack]
tags: [Programmers,Stack]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -스택/큐 - 주식 가격 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/SQ_01/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
스택 큐 Level 2 문제 입니다.    

-----  

## 3. 생각한 것들(문제 접근 방법)

- 입력 크기가 최대 10만으로 for문 2개를 사용해서 각각 인덱스에 대해 떨어지는 지점을 찾아주는 것은 시간초과가 날 것이라 생각했습니다.
- 하나하나 탐색하는 것은 분명 해야하는 일이나 한번에 처리할 수 있는 것이 필요했습니다. 
- **증가하는 시점**은 고려할 필요가 없습니다. 최대한 무시하려고 했습니다.
- 이렇게 생각하다보니 떨어지는 시점인 방금 들어온값에 비해 더 작은 값이 들어오려하면 막는 코드를 짜려 하다보니 Stack을 사용하게 되었습니다.
- 스택의 첫 번째 요소에는 '주식 가격' 두번째 요소에는 '인덱스 값'을 저장하였습니다. 이렇게하면 비교는 주식가격으로 하고, 빼면서 인덱스 값으로 임시 배열에 넣으므로 불필요한 연산들을 줄일 수 있다고 생각했습니다.

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <stack>
using namespace std;

vector<int> solution(vector<int> prices) {
    vector<int> answer;
    int temp[100001];
    // 첫번째는 가격, 두번째 인덱스
    stack<pair<int, int>> s;

    for (size_t i = 0; i < prices.size(); ++i)
    {
        //배열을 다 돌지 않았는데 스택이 비어있는 경우 그냥 넣습니다.
        if (s.empty())
            s.push(make_pair(prices[i], i));
        // 새로 들어오려는 값이 더 큰 경우(무시해도 되는 경우) 그냥 넣습니다.
        else if (s.top().first <= prices[i])
        {
            s.push(make_pair(prices[i], i));
        }
        //들어오는 게 더 작은 경우
        else
        {
            //스택 꼭대기와 비교하여 같거나 작은값이 위에 올 때까지 스택에서 뺍니다. 임시 배열에 temp[결과 인덱스] = 뺀 만큼 을 넣습니다.
            while (!s.empty() && s.top().first > prices[i])
            {
                int result = i - s.top().second;
                temp[s.top().second] = result;
                s.pop();
            }
            s.push(make_pair(prices[i], i));
        }
    }
    //스택이 비어있지 않은 경우 남아있는 것들은 모두 증가상태이므로 빼면서 임시 배열에 결과값을 넣어줍니다.
    while (!s.empty())
    {
        temp[s.top().second] = prices.size() - s.top().second - 1;
        s.pop();
    }
    //임시 배열을 돌면서 answer벡터에 넣어줍니다.
    for (int i = 0; i < prices.size(); ++i)
    {
        answer.push_back(temp[i]);
    }

    return answer;
}
```
-----

## 5. 결과
생각하기 너무 어렵네요.

![](/assets/img/sample/Programmers/SQ_01/result.JPG)











 