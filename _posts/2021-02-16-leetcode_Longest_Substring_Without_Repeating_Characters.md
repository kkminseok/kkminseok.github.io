---
title: leetcode(리트코드)3-Longest Substring Without Repeating Characters
author: 강민석
date: 2021-02-16 06:10:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 3 - Longest Substring without Repeating Characters 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/longest-substring-without-repeating-characters/>  

![](/assets/img/sample/leetcode/3/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/3/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked의 문제입니다.  


-----  

## 4. 문제 해석

- 중복되는 문자가 없는 가장 큰 문자열의 길이를 찾습니다. 


-----  

## 5. code

**c++**


```c++
class Solution {
public:
    bool checkalpha[128]={false,};
    int lengthOfLongestSubstring(string s) {
        int count = 0;
        int result = 0;
        for(size_t i=0;i<s.length();++i)
        {
            memset(checkalpha,false,sizeof(checkalpha));
            string temp = "";
            temp+=s[i];
            count = 1;
            checkalpha[s[i]]=true;
            for(size_t j =i+1;j<s.length();++j)
            {
                if(checkalpha[s[j]]==true)
                    break;
                else
                {
                    temp+=s[j];
                    ++count;
                    checkalpha[s[j]]=true;
                }
            }
            result = max(result,count);
        }
        return result;
    }
};
```

-----  

**python**

밑의 개선 코드를 python으로 바꾸었습니다.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        umap = {}
        maxlen = 0
        left = 0 
        
        for right in range (0,len(s)):
            if umap.get(s[right]) != None:
                left = max(left,umap.get(s[right]) + 1)
                    
            umap[s[right]] =right
            maxlen = max(maxlen,right-left+1)
        return maxlen
     
```


-----

## 6. 결과 및 후기, 개선점

**시간(32%)**  

![](/assets/img/sample/leetcode/3/result.JPG)


**4ms 코드(92%)**
```c++
class Solution {
public:
    struct node{
        int val = -1;
    };
    int lengthOfLongestSubstring(string s) {
        int maxlen = 0;
        //해시맵을 선언합니다.
        unordered_map<char,node> umap;
        
        for(int right = 0, left = 0; right < s.length(); right++){
            // Update left and right ptr
            //만약 right값이 이미 중복되어 들어온 값이라면 left를 바꿔줍니다. 다시 세줘야하기 때문
            if(umap[s[right]].val != -1){
                left = max(left,umap[s[right]].val + 1);
            }
            // Update the map
            //right값을 갱신합니다.
            umap[s[right]].val = right;
            // Calculate length til now - Local and global length
            maxlen = max(maxlen,right-left+1);
        }
        
        return maxlen;
    }
};
```

아무래도 2중 for문이 시간을 많이 잡아먹은 것 같습니다.  