---
title: leetcode(리트코드)49-Group Anagrams
author: 강민석
date: 2021-02-25 14:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 49 - Group Anagrams 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/group-anagrams/>  

![](/assets/img/sample/leetcode/49/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/49/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 문자열로 들어온 것들을 순서를 바꾸어도 같은것 끼리 뭉쳐서 리턴합니다.

-----  

## 5. code

```c++
bool cmp(pair<string,int>& a,pair<string,int>& b)
{
    return a.first<b.first;
}
class Solution {
public:

    
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        //첫번째는 기존 str, 두번째는 인덱스 값을 저장
        vector<pair<string,int>> vec;
        //결과 벡터
        vector<vector<string>> result;
        //strs를 돌면서 strs[i]를 정렬해주고 인덱스와 함께 넣습니다.
        for(size_t i =0;i<strs.size();++i)
        {
            string temp = strs[i];
            sort(temp.begin(),temp.end());
            vec.push_back(make_pair(temp,i));
        }
        // 첫번째 값을 기준으로 정렬합니다.
        sort(vec.begin(),vec.end(),cmp);
        for(size_t i = 0; i<vec.size();++i)
        {
            vector<string> tempvec;
            tempvec.push_back(strs[vec[i].second]);
            //다음 요소가 같으면 몰아서 push하빈다.
            while(i+1<vec.size() && vec[i+1].first == vec[i].first)
            {
                ++i;
                tempvec.push_back(strs[vec[i].second]);
            }
            result.push_back(tempvec);
        }
    
       return result;
    }
};
```


-----

## 6. 결과 및 후기, 개선점



![](/assets/img/sample/leetcode/49/result.JPG)  







