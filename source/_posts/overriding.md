---
title: overriding
date: 2021-03-28 14:34:16
tags: C++
categories:
    - C++
---

## 함수 오버라이딩

- 개와 고양이 클래스를 따로 생성하여 동물 클래스에 상속받기
- 울음소리를 오버라이딩

```c++
#include <iostream>

using namespace std;

class Animal
{
public:
    void cry()
    {
        cout << "짖는소리" << endl;
    }
};

class Dog : public Animal
{
public:
    void cry()
    {
        cout << "개짖는소리 왈왈" << endl;
    }
};

class Cat : public Animal
{

public:
    void cry()
    {
        cout << "고양이 짖는소리 냐옹" << endl;
    }
};



int main()
{
    Dog d;
    Cat c;

    d.cry();
    c.cry();

    return 0;
}
```
