---
title: leetcode(리트코드)146-LRU Cache
author: 강민석
date: 2021-03-21 05:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 146 - LRU Cache 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/lru-cache/>  

![](/assets/img/sample/leetcode/146/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/146/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 문제 내용은 어렵지 않습니다. LRU를 기준으로put, get함수를 구현하는 것입니다.
- put과 get이 호출될 때마다 '최근' 사용된 것으로 봅니다.
- 같은 키값이 들온 경우 해당 키값의 value를 수정해줘야합니다.
- 개인적으로 굉장히 좋은 문제라고 생각합니다.!! Discuss를 봤지만, 해당 코드를 이해하기 위해 공책으로 리스트와 맵의 구조를 몇 번 그렸는 지 모르겠습니다. 



-----  

## 5. code


**c++**

```c++
class LRUCache {
    typedef unordered_map<int,pair<int,list<int>::iterator>> UMIL;
    UMIL cache;
    list<int> result;
    int _size;
public:
    LRUCache(int capacity) {
        _size = capacity;
    }
    
    int get(int key) {
        auto it = cache.find(key);
        if(it!=cache.end())
        {
            move(it);
            return it->second.first;
        }
        else
            return -1;
    }
    
    void put(int key, int value) {
        auto it = cache.find(key);
        if(it!=cache.end())
        {
            //수정
            move(it);
        }
        else
        {
            if(_size == cache.size())
            {
                cache.erase(result.back());
                result.pop_back();
            }
            result.push_front(key);
        }
        cache[key] = {value, result.begin()};
            
    }
    void move(UMIL::iterator it)
    {
        int key = it->first;
        result.push_front(key);
        result.erase(it->second.second);
        it->second.second = result.begin();
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 56% python ??%** 
![](/assets/img/sample/leetcode/146/result.JPG)  






