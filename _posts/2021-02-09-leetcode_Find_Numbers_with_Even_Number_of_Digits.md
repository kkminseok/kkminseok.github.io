---
title: leetcode(리트코드)1295-Find Numbers with Even Number of Digits
author: 강민석
date: 2021-02-09 13:02:00 +0800
categories: [leetcode,Eazy]
tags: [leetcode,Array]
math: true
mermaid: true
image: 
comments: true
---

**leetcode Array intro - Find Numbers with Even Number of Digits 문제입니다.**

## 1. 문제
<https://leetcode.com/explore/learn/card/fun-with-arrays/521/introduction/3237/>  

![](/assets/img/sample/leetcode/3237/Problem.JPG)

-----  

## 2. Input , Output

-----  

## 3. 분류 및 난이도

leetcode의 Array introduction에 있는 문제입니다.  

어렵지 않고, leetcode가 어떤 시스템인지 인지하기 위해 풀었습니다.  



-----  

## 4. 문제 해석

- 배열을 돌면서 짝수크기인 배열요소를 찾아줍니다. 예를 들어 12는 크기가 2이므로 카운트를 세주고 7896도 크기가 4이므로 카운트를 세줍니다.


-----  

## 5. code

```c++
class Solution {
public:
    int findNumbers(vector<int>& nums) {
        //정렬
        sort(nums.begin(),nums.end());
        int count=0;
        int division;
        int temp=nums[0];
        int init=0;
        while(temp!=0)
        {
            temp/=10;
            init++;
        }
        //맨처음 요소가 짝수이면 10, 1000,10000 등으로 맞추려고 함.
        if(init%2==0)
            --init;
        division = pow(10,init);
        
        for(size_t i=0;i<nums.size();)
        {
            //홀수라면 넘어감.
            if(division > nums[i])
            {
                ++i;
                continue;
            }
            else
            {
                division*=10;
                //짝수라면 그 범위내에서 카운트를 세줌. 10~99, 1000~9999 ...
                while(i<nums.size() && nums[i]<division&&nums[i]>=division/10)
                {
                    ++count;
                    ++i;
                }
                division*=10;
            }
        }
        return count;
    }
};
```
-----

## 6. 결과 및 후기, 개선점

**시간**  
참고로 시간은 제출할 때마다 다르다고 합니다.  
이유 : <https://leetcode.com/discuss/general-discussion/678668/why-does-the-runtime-change-every-time-the-code-is-submitted>  
요약 하자면 사람들이 동 시간대에 제출을 많이하면 할수록 런타임이 길어진다고 합니다.  
알고리즘이 차이가 나지않는 이상 거의 변동이 없다고 합니다.  


![](/assets/img/sample/leetcode/3237/result1.JPG)  

**메모리**
![](/assets/img/sample/leetcode/3237/result2.JPG)  



**시간효율성이 좋은 코드**


```c++
typedef long long ll;
#define rep(i,s,n)for(ll i = s;i<n;i++)
///////////////////////////////////////////////////////////
class Solution {
public:
    // 숫자의 자릿수를 세주는 함수.
	int digits(int n) {
		int d = 0;
		while (n) {
			n /= 10;
			d++;
		}
		return d;
	}
	int findNumbers(vector<int>& n) {
		int ans = 0;
		rep(i, 0, n.size()) {
            //숫자 자릿수가 짝수라면 ans에 더함.
			ans += (digits(n[i]) % 2 == 0);
		}
		return ans;
	}
};
```
  
**공간 효율성이 좋은 코드** 
 
```c++
class Solution {
public:
    int findNumbers(vector<int>& nums) {
        int count = 0;
        for(int i = 0;i<nums.size();i++)
        {
            //string으로 바꿔버림
            string conv = to_string(nums[i]);
            //string의 크기가 짝수이면 count를 더함.
            if(conv.size() % 2 == 0)
            {
                count += 1;
            }
        }
        return count;
    }
};
```

스트링으로 바꾼다는 생각을 했는데.. 좀 더 생각할걸..