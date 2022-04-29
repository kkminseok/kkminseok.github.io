---
title: leetcode(리트코드)208-Implement Trie (Prefix Tree)
author: 강민석
date: 2021-03-26 04:00:00 +0800
categories: [leetcode,Medium]
tags: [leetcode,Top100Like]
math: true
mermaid: true
image: 
comments: true
---

**leetcode 208 - Implement Trie (Prefix Tree) 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/implement-trie-prefix-tree/>  

![](/assets/img/sample/leetcode/208/Problem.JPG)

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/208/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도 문제입니다.  
leetcode Top 100 Liked 문제입니다.  


-----  

## 4. 문제 해석

- 자신만의 자료구조를 만들어야합니다.
- serach()함수는 해당 자료구조에 있는 지 결과값을 리턴합니다.
- startWith() 함수는 들어온 문자열이 본문에 접두사로 존재하는 지 확인합니다.
- startWtih() 함수를 작성하기 위해 들어온 문자열에 각각의 접두사를 전부 map에다 넣어줬습니다. 그래서 느린 코드가 완성된 것 같습니다.



-----  

## 5. code


**c++**

```c++
class Trie {
public:
    /** Initialize your data structure here. */
    unordered_map<string,int> um;
    unordered_map<string,int> preum;
    Trie() {

    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        um[word]=  1;
        string temp = "";
        for(size_t i = 0;i<word.size();++i)
        {
            temp += word[i];
            preum[temp] = 1;
        }
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        auto it = um.find(word);
        if(it==um.end())
            return false;
        return true;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        auto it = preum.find(prefix);
        if(it==um.end())
            return false;
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```


-----

## 6. 결과 및 후기, 개선점

코드에 대한 설명이 필요하신 분은 댓글을 달아주세요.!!

**c++ 21% python ??%** 
![](/assets/img/sample/leetcode/208/result.JPG)  



**c++28ms 100% code**


```c++
#define MAX_NODES 10000
class Trie {
    struct Trienode
    {
        char val;
        int count;
        int endsHere;
        Trienode *child[26];
    };
    Trienode *root;
    
    Trienode *getNode(int index)
    {
        Trienode *newnode = new Trienode;
        newnode->val = 'a'+index;
        newnode->count = newnode->endsHere = 0;
        for(int i=0;i<26;++i)
            newnode->child[i] = NULL;
        return newnode;
    }
public:
    /** Initialize your data structure here. */
    Trie() {
        ios_base::sync_with_stdio(false);
        cin.tie(NULL);
        root = getNode('/'-'a');
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Trienode *curr = root;
        int index;
        for(int i=0;i<word.length();i++)
        {
            index = word[i]-'a';
            if(curr->child[index]==NULL)
                curr->child[index] = getNode(index);
            curr->child[index]->count +=1;
            curr = curr->child[index];
        }
        curr->endsHere +=1;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Trienode *curr = root;
        int index;
        for(int i=0;i<word.length();i++)
        {
            index = word[i]-'a';
            if(curr->child[index]==NULL)
                return false;
            curr = curr->child[index];
        }
        if((curr->endsHere)>0)
            return true;
        return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Trienode *curr = root;
        int index;
        for(int i=0;i<prefix.length();i++)
        {
            index = prefix[i]-'a';
            if(curr->child[index]==NULL)
                return false;
            curr = curr->child[index];
        }
        
        if((curr->count)>0)
            return true;
        return false;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

