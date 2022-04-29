---
title: leetcode(리트코드)139-Word Break
author: 강민석
date: 2021-03-17 02:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 139 - Word Break 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/word-break/>  

![](/assets/img/sample/leetcode/139/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/139/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- s의 문자열과 wordDict이라는 문자열이 들어옵니다.
- wordDict의 문자들을 사용 및 중복사용하여, s문자열을 만들 수 있으면 True, 만들 수 없으면 False를 리턴합니다.
- wordDict을 중복사용할 수 있어서 고민을 많이 했는데 c++은 map를 이용하고, python은 'in' 구문을 사용하여 구별할 수 있었습니다.
- 시작 인덱스만 조회하여, 끝까지 닿을 수 있으면 True, 닿을 수 없으면 False를 리턴합니다.
-----  

## 5. code


**c++**

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        bool* v = new bool[s.size()+1];
        memset(v,false,sizeof(bool) *(s.size()+1) );
        v[0] = true;
        unordered_map<string,bool> map;
        for(size_t i =0;i<wordDict.size();++i)
        {
            map[wordDict[i]] = true;
        }
        for(int i =1;i<s.size()+1;++i)
        {
            for(int j =0;j<i;++j)
            {
                string temp(s,j,i-j);
                if(v[j] == true)
                {
                    auto it = map.find(temp);
                    if(it!=map.end())
                    {
                        v[i]=true;
                        break;
                    }
                }
            }
        }
        return v[s.size()];
    }
};
```

-----  

**python**

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        v = [0] * (len(s)+1)
        v[0]=1
        for i in range(1,len(s)+1):
            for j in range(i) : 
                if v[j] ==1 and s[j:i] in wordDict:
                    v[i] = 1
                    break
        if v[len(s)] == 1:
            return True
        return False
            
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 45% python 83%** 
![](/assets/img/sample/leetcode/139/result.JPG)  




**c++ 0ms 100% code**

```c++
//dp
class Solution
{
 public:
 bool wordBreak(string s, vector<string>& wordDict) {
        if (wordDict.size() == 0) return false;
        
        unordered_set<string> d(wordDict.begin(), wordDict.end());
           
		 // dp[i] is true only if a valid word or sequence of words ends at i
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        
        for (int i = 1; i <= s.size(); ++i) {
            for (int j = i - 1; j >= 0; --j) {
                // check only if a valid sequence of words (or a word) ends at j
                if (dp[j]) {
                    const string sub = s.substr(j, i - j);
                    if (d.count(sub)) {
                        // Ending at i is a valid word
                        dp[i] = true; 
						// Others j values might be false
						// We stop here since there is one valid sequence ending here
                        break; 
                    }
                }
            }
        }
        return dp[s.size()];
    }
};
```

로직은 동일한 것 같은데 시간차이가 많이 납니다.

