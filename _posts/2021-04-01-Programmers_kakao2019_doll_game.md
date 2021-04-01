---
title: Programmers_카카오2019겨울 인턴십 - 크레인 인형 뽑기
author: 강민석
date: 2021-04-01 13:30:40 +0800
categories: [Algorithm,Programmers]
tags: [Programmers,kakao2019ship]
math: true
mermaid: true
image: 
comments: true
---

**프로그래머스 - 크레인 인형 뽑기 문제 입니다.**

## 1. 문제
<https://programmers.co.kr/learn/courses/30/lessons/64061>






-----  

## 2. 분류 및 난이도

Programmers 문제입니다.  
2019년도 kakao 겨울 인턴십 문제이고, 난이도는 가장 쉬운 난이도입니다.


-----  

## 3. 생각한 것들(문제 접근 방법)

- 인형뽑기를 하여 바구니에 넣는데 바구니 맨 위의 값과 같으면 인형을 집은 인형과 바구니의 인형을 뺍니다.
- 뺀 인형의 개수를 구합니다.
- stack을 이용하라고 문제에서 대놓고 알려줘서 어렵지 않습니다.



-----  

## 4. 접근 방법을 적용한 코드

```c++
#include <string>
#include <vector>
#include <stack>
#include <iostream>
using namespace std;

int solution(vector<vector<int>> board, vector<int> moves) {
    int answer = 0;
    stack<int> st;
    int row = board.size();
    for(int i = 0;i<moves.size();++i){
        int index = moves[i]-1;
        int count = 0;
        while(count < row && board[count][index]==0){
            ++count;
        }
        if(count==row) continue;
        
        int find = board[count][index];
        board[count][index]=0;
        if(st.empty()) st.push(find);
        else{
            if(st.top() == find) answer+=2,st.pop();
            else
                st.push(find);
        }
        
    }
    return answer;
}
```

-----

## 5. 결과

필요시.














 
