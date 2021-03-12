---
title: leetcode(리트코드)3월12일 challenge1461-Check If a String Contains All Binary Codes of Size K
author: 강민석
date: 2021-03-12 16:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 12일 - Check If a String Contains All Binary Codes of Size K 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/>  

![](/assets/img/sample/leetcode/1461/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/1461/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 12일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- string 으로 들어온 s안에 k만큼의 크기를 가진 binary 숫자들이 전부 들어 있는 지 확인합니다.
- set은 중복 값이 들어오거나 값이 들어오지 않은 경우는 크기가 커지지 않고 모든 경우의 수는 2^k 이므로 두 값을 비교하였습니다.




-----  

## 5. code

**python**

```python
class Solution:
    def hasAllCodes(self, s: str, k: int) -> bool:
        sh = set()
        if k> len(s):
            return False
        
        for i in range(len(s)-k+1):
            temp = str()
            for j in range(k):
                temp +=s[i+j]
            sh.add(temp)
        if len(sh) == pow(2,k):
            return True
        return False
        
        
        
```

------

**c++**

```c++
class Solution {
public:
    bool hasAllCodes(string s, int k) {
        unordered_set<string> sh;
        if(k>s.size())
            return false;
        
        for(int i=0;i<=s.size()-k;++i)
        {
            string temp = "";
            for(int j =0;j<k;++j)
            {
                temp+=s[i+j];
            }
            //cout<<temp<<" "<<sh.size()<<'\n';
            sh.insert(temp);
        }
        
        if(sh.size() == pow(2,k))
            return true;
        
        return false;
    }
};
```

-----

## 6. 결과 및 후기, 개선점

**c++ 40% python 14%**

![](/assets/img/sample/leetcode/1461/result.JPG)  


**c++ 100% 12ms code**

```c++
class Solution {
public:
	bool hasAllCodes(string& s, int k) {
        
        if (k>15) return false;
		
		bool set[1<<k];
        memset(set, 0, sizeof(set));
		
		int n = stoi(s.substr(0,k), 0, 2);
		for (int i=k; i <= s.size(); i++) {
			// check prev window
			set[n] = true;
			if (i == s.size()) break;
            n = n & ~(1 << (k-1)); // clear kth bit
			n = (n << 1) | (s[i]-'0');
		}
        
        for (int i=0; i < (1<<k); i++)
            if (set[i] == false) return false;
		
		return true;
	}
	
};

static auto _______ = [](){
	std::ios::sync_with_stdio(false);
	std::cin.tie(NULL);
	std::cout.tie(NULL);
	return 0;
}();
```

**python 224ms 96% code**

```python
class Solution:
    def hasAllCodes(self, s: str, k: int) -> bool:
        count = 2 ** k
        seen = set()
        for i in range(k, len(s)+1):
            temp = s[i-k:i]
            if temp not in seen:
                seen.add(temp)
                count -= 1
                if count == 0:
                    return True
        return False
```

제가 파이썬이 미성숙해 temp를 업데이트하는 for문을 돌지 않았다는 점에서 다릅니다.
