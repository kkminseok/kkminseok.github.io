---
title: leetcode(리트코드)2월08일 challenge284-Peeking Iterator
author: 강민석
date: 2021-02-16 15:20:00 +0800
categories: [leetcode,Medium]
tags: [leetcode]
math: true
mermaid: true
image: 
comments: true
---

**leetcode February challenge08 - Peeking Iterator 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/peeking-iterator/>  

![](/assets/img/sample/leetcode/284/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/284/input.JPG)  

-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
2월08일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 함수를 구현하는 문제인데, 동작방식에 대한 설명이 불친절한 것 같습니다. Discuss를 보고 이해했습니다.

-----  

## 5. code

```c++
/*
 * Below is the interface for Iterator, which is already defined for you.
 * **DO NOT** modify the interface for Iterator.
 *
 *  class Iterator {
 *		struct Data;
 * 		Data* data;
 *  public:
 *		Iterator(const vector<int>& nums);
 * 		Iterator(const Iterator& iter);
 *
 * 		// Returns the next element in the iteration.
 *		int next();
 *
 *		// Returns true if the iteration has more elements.
 *		bool hasNext() const;
 *	};
 */

class PeekingIterator : public Iterator {
public:
	PeekingIterator(const vector<int>& nums) : Iterator(nums) {
	    // Initialize any member here.
	    // **DO NOT** save a copy of nums and manipulate it directly.
	    // You should only use the Iterator interface methods.
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	int peek() {
        
        return Iterator(*this).next();
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	int next() {
        return Iterator::next();
	}
	
	bool hasNext() const {
	    
        return Iterator::hasNext();
	}
};
```


-----

## 6. 결과 및 후기, 개선점
  
Discuss를 봤으므로 적지 않습니다.  


