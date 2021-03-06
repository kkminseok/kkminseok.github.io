---
title: leetcode(리트코드)6-ZigZag Conversion
author: 강민석
date: 2021-02-16 16:10:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 6 - ZigZag Conversion 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/zigzag-conversion/>  

![](/assets/img/sample/leetcode/6/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/6/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 규칙을 이용해서 만든 문자열을 리턴합니다.
- 브루트 포스로는 힘들어보이고, 규칙이 있어서 그것을 이용합니다.
- 규칙은 Nums이 4가 들어올 경우 첫줄은 6 0 / 두번째 줄 4 2 / 세번째줄 2 4 / 네번째줄 0 6 이렇게 인덱스가 이동하고 Nums가 5인 경우 8 0 / 6 2 / 4 4 / 2 6 / 0 8 이런식으로 인덱스가 이동합니다.



-----  

## 5. code

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        //iter는 인덱스를 움직이기 위한 초기값입니다.
        int iter = (numRows-1)*2;
        //numRows가 1이거나 문자열보다 크면 그냥 문자열을 반환해야합니다.
        if(numRows==1 || numRows > s.size())
            return s;
        string result="";
        for(int i = 0;i<numRows;++i)
        {
            //처음 접근하는 numRows는 넣어줍니다.
            result+=s[i];
            // right는 왼쪽의 규칙성입니다. !8! 0 !6! 2.. 이런식으로 동작합니다. 코드를 쓰고나니 left와 right를 반대로 썼네요..
            int right = iter-(i*2);
            // left는 right와 반대입니다. 8 !0! 6 !2! 이런식으로 동작합니다.
            int left = iter-right;
            //초기 인덱스값입니다.
            int index = i;
            //문자열을 넘지 않도록 잘 조절합니다.
            for(;index<s.size();)
            {
                //항상 넘지 않도록 조건문을 달아줍니다.
                if(s.size()<=index+right)
                    break;
                //0인경우는 더해봤자 중복값이 들어가므로 무시합니다.
                if(right!=0 && s[index+right]!=' ')
                    result+=s[index+right];
                index+=right;
                if(s.size()<=index+left)
                    break;
                if(left!=0 && s[index+left]!=' ')
                {
                    result+=s[index+left];
                }
                index+=left;
            }
        }
        return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점

**시간(89%)**  

![](/assets/img/sample/leetcode/6/result.JPG)


**시간 0ms(100%)코드**

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        //크기가 1인 경우 리턴
        if( 1 == numRows) return s;
        
        size_t input_size = s.size();
        string result(input_size,0);
        
        //functionSwitch는 어느 시점에서 배열의 인덱스를 넘을 지 모르므로 관리하는 boolean타입의 변수입니다.
        bool functionSwitch = true;
        //output_counter는 s의 인덱스를 관리합니다.
        size_t output_counter = 0;
        //input_counter는 result의 인덱스를 관리합니다.
        size_t input_counter = 0;
        
        for(int row_counter = 1; row_counter <= numRows; row_counter++)
        {
            //첫 번째 규칙을 적굥합니다.
            functionSwitch = true;
            output_counter = row_counter - 1;
            
            while(output_counter < input_size)
            {
                //첫 번째 규칙
                if(functionSwitch)
                {
                    result[input_counter] = s[output_counter];
                    functionSwitch = false;
                    
                    // 위에서 설명한 numRows와 같은 경우는 중복값을 저장할 수 있으므로 생략합니다.
                    if(row_counter == numRows) continue;
                    //규칙
                    output_counter += 2*(numRows - row_counter);
                }
                else
                {
                    result[input_counter] = s[output_counter];
                    functionSwitch = true;
                    
                    //중복값 무시
                    if(row_counter == 1) continue;
                    //규칙
                    output_counter += 2*(row_counter - 1);
                }
                
                input_counter++;
            }
        }
        return result;
    }
};
```