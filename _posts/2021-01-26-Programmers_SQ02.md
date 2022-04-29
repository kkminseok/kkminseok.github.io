---
title: Programmers_Stack/Queue02 - 기능개발
author: 강민석
date: 2021-01-26 15:03:00 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,Queue]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 -스택/큐 - 기능개발 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/SQ_02/Problem.JPG)  


-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
스택 큐 Level 2 문제 입니다.    

-----  

## 3. 생각한 것들(문제 접근 방법)

- 입력의 크기가 크지도 않아서 효율성은 고려 안했습니다.
- 앞에서부터 선행처리가 되어야하므로 Queue를 생각했습니다.
- 간단하게 day라는 것을두고 day를 증가시키면서 앞에서부터 progress가 완료되었으면 pop()하는 방식으로 풀었습니다.
- 마지막에 q.pop()를 하면서 while문을 빠져나가버려서 불 필요한 코드를 넣게 된 거 같네요. 


-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<queue>

using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer;
    queue<pair<int, int>> q;
    for (size_t i = 0; i < progresses.size(); ++i)
    {
        q.push(make_pair(100 - progresses[i], speeds[i]));
    }
    int day = 1;
    int count = 0;
    while (!q.empty())
    {
        int progress = q.front().first;
        int speed = q.front().second;
        if (progress - (day * speed) <= 0)
        {
            ++count;
            //이 부분 없으면 while문 빠져나와서 마지막에 계산안해요.
            if (q.size() == 1)
            {
                answer.push_back(count);
            }
            q.pop();
        }
        else
        {
            day++;
            if (count != 0)
            {
                answer.push_back(count);
                count = 0;
            }
        }
    }

    return answer;
}
```
-----

## 5. 결과

![](/assets/img/sample/Programmers/SQ_02/result.JPG)











 