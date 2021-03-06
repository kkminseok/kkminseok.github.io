---
title: Programmers_Greedy02 - 조이스틱
author: 강민석
date: 2021-02-01 13:06:00 +0800
categories: [Algorithm,Greedy]
tags: [Programmers,Greedy]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 조이스틱 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/parts/12077>
![](/assets/img/sample/Programmers/Greedy02/Problem.JPG)  
![](/assets/img/sample/Programmers/Greedy02/Problem2.JPG)  

-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
level 2의 문제입니다.  

-----  

## 3. 생각한 것들(문제 접근 방법)

- 두번째 인덱스가 A인경우만 검사해주면 되는 줄 알았습니다.
- 테스트케이스 11에서 계속 틀려서 애를 많이 먹었습니다.
- 보아하니 BBBAAAB같은 예제는 BBB간다음 되돌아가서 맨 끝을 B로 만들어주는게 최선의 정답인 경우가 있었습니다.
- 다른 사람들의 코드를 보며 이해한대로 다시 코딩하였습니다.

**틀린 코드**
```c++
#include <string>
#include <vector>
#include<iostream>
using namespace std;

int solution(string name) {
    int answer = 0;
    size_t size = name.size();

    size_t index = 0;
    for (; index < size; ++index)
    {
        if (index == 1 && name[index] == 'A')
        {
            while (name[index] == 'A')
                ++index;
        }
        if (name[index] - 65 > 13)
        {
            cout << answer << name[index] << "1" << '\n';
            answer += 91 - name[index];
            cout << 91 - name[index] << "asdsad";
        }
        else
        {
            cout << answer << name[index] << "2" << '\n';
            answer += name[index] - 65;
            cout << name[index] - 65 << "qqqqqq";
        }
        ++answer;
    }
    //마지막에 인덱스를 옮긴것도 포함이 되므로
    return answer - 1;
}
```

-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include<iostream>
using namespace std;

int solution(string name) {
    int answer = 0;
    //현재 어디인지 가르키는 변수
    int move = 0;
    //A로 초기화
    string compare(name.length(), 'A');
    while (compare != name)
    {
        //next는 다음인덱스를 왼쪽부터 돌 지 오른쪽부터 돌 지 정하는 변수
        int next = 0; int left = 0; int right = 0;
        for (int i = 0; i < name.size(); ++i)
        {
            if (move + i < name.size()) right = move + i;
            else  right = move + i - name.size();

            if (move - i >= 0) left = move - i;
            else left = move - i + name.size();

            //오른쪽으로 가야하는 경우가 우선순위를 가집니다.
            if (compare[right] != name[right]) next = right;
            else if (compare[left] != name[left])next = left;
            else
                continue;


            answer += i;
            //조이스틱 위로 누른게 빠른지, 밑으로 누른게 빠른지
            answer += min(name[next] - 'A', 'Z' - name[next] + 1);
            //값을 바꿔줌.
            compare[next] = name[next];
            break;
        }
        move = next;
    }

    return answer;
}
```
-----

## 5. 결과

테스트케이스 11때문에 너무 어려웠습니다.  














 
