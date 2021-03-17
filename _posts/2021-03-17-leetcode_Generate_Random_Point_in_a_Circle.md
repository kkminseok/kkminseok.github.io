---
title: leetcode(리트코드)3월17일 challenge478-Generate Random Point in a Circle
author: 강민석
date: 2021-03-17 05:03:20 +0800
categories: [leetcode,Medium]
tags: [leetcode,AlgorithmStudy]
math: true
mermaid: true
image: 
comments: true
---

**leetcode March 17일 - Generate Random Point in a Circle 문제입니다.**

## 1. 문제
<https://leetcode.com/problems/generate-random-point-in-a-circle/>  

![](/assets/img/sample/leetcode/478/Problem.JPG)  

-----  

## 2. Input , Output

![](/assets/img/sample/leetcode/478/input.JPG)  


-----  

## 3. 분류 및 난이도

Medium 난이도입니다.  
3월 17일자 챌린지 문제입니다. 

-----  

## 4. 문제 해석

- 랜덤으로 원의 좌표를 찍는 것인데, 테스트 케이스와 값이 같아야합니다.
- 좋은 문제는 아닌듯 합니다.


-----  

## 5. code

**c++ 삽질한 코드 테스트 케이스 답이 정해져있는 줄 몰랐다.**


```c++
class Solution {
private:
    double cradius;
    double cx;
    double cy;
public:
    Solution(double radius, double x_center, double y_center) {
        this->cradius = radius;
        this->cx = x_center;
        this->cy = y_center;
    }
    
    vector<double> randPoint() {
     vector<double> result;
          // 시드값을 얻기 위한 random_device 생성.
      std::random_device rd;

      // random_device 를 통해 난수 생성 엔진을 초기화 한다.
      std::mt19937 gen(rd());

      // 0 부터 99 까지 균등하게 나타나는 난수열을 생성하기 위해 균등 분포 정의.
        //x 먼저
        std::uniform_real_distribution<> dis(cx-cradius,cx+cradius);
        double rx=dis(gen);
        //y의 범위  x^2 + y^2 <= r^2 이므로 
        double tempyf = sqrt(cradius * cradius - rx * rx);
        cout<<tempyf<<"tempyf";
        std::uniform_real_distribution<> dis2(0.0,tempyf);
        double ry =dis2(gen);
        
        cout<<rx<<" "<<ry<<'\n';
        result.push_back(rx);
        result.push_back(ry);
        return result;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(radius, x_center, y_center);
 * vector<double> param_1 = obj->randPoint();
 */
```

-----  

**정답인 c++코드**

```c++
class Solution {
public:
    double radius;
    double x_center;
    double y_center;
    
    Solution(double radius, double x_center, double y_center) {
        this->radius = radius;
        this->x_center = x_center;
        this->y_center = y_center;
    }
    
    vector<double> randPoint() {
        double x;
        double y;
        
        do {
            x = (2 * ((double)rand() / RAND_MAX) - 1.0) * radius;
            y = (2 * ((double)rand() / RAND_MAX) - 1.0) * radius;
        } while (x * x + y * y > radius * radius);
        
        return { x_center + x, y_center + y };
    }
};
```

-----

## 6. 결과 및 후기, 개선점

skip