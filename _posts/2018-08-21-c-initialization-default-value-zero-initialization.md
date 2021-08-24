---
layout: post
ads: true
comments: true
published: true
title: 'C++ initialization - default,value,zero-initialization'
categories:
  - program
tags:
  - C++
  - C++ initialization
---
#### Trivial vs. standard layout vs. POD
Ref:
- [Generalized Plain Old Data](http://www.modernescpp.com/index.php/generalized-plain-old-data)
- [Trivial vs. standard layout vs. POD](https://stackoverflow.com/a/6496703)
- [POD-Plain Old Data](https://stackoverflow.com/a/12979623/5715800)

#### Explicitly Defaulted and Deleted Functions in C++ 11
Ref:
- [Explicitly Defaulted and Deleted Functions in C++ 11](https://www.geeksforgeeks.org/explicitly-defaulted-deleted-functions-c-11/)
- [Default constructors](https://en.cppreference.com/w/cpp/language/default_constructor)

#### C++ initialization - default,value,zero-initialization
Ref:
- [Initialization in C++ is bonkers](https://blog.tartanllama.xyz/initialization-is-bonkers/)

#### Example
```cpp
/*
file: initialization.cpp
brief: demo c++/c++11 initialization rules
compile/run: g++ -g -std=c++11 initialization.cpp -o initialization && ./initialization
*/

#include <iostream>

using namespace std;

class AKlass
{
private:
public:
  AKlass(){};
  ~AKlass(){};
};

AKlass global; // zero-initialization, then default-initialization

void
foo_func()
{
  AKlass i;            // default-initialization
  AKlass j{};          // value-initialization (C++11)
  AKlass k = AKlass(); // value-initialization
  AKlass l = AKlass{}; // value-initialization (C++11)
  AKlass m();          // function-declaration

  new AKlass;   // default-initialization
  new AKlass(); // value-initialization
  new AKlass{}; // value-initialization (C++11)
}

struct A
{
  AKlass t;
  A()
    : t()
  {}
}; // t is value-initialized
struct B
{
  AKlass t;
  B()
    : t{}
  {}
}; // t is value-initialized (C++11)
struct C
{
  AKlass t;
  C() {} // empty constructor
};       // t is default-initialized


// Example
struct foo
{
  foo() = default; // defaulted default constructor
  int a;
};

struct bar
{
  bar();
  int b;
};

bar::bar() = default; // treated as user-provided

foo foo_global;
bar bar_global;

int
main()
{
  cout << "foo_global: " << foo_global.a << " bar_global: " << bar_global.b
       << endl;

  foo default_init;
  foo value_init = foo();
  foo cpp11_value_init{};
  foo cpp11_value_init2 = foo{};
  std::cout << "default_init: " << default_init.a
            << " value_init: " << value_init.a
            << " cpp11_value_init: " << cpp11_value_init.a
            << " cpp11_value_init2: " << cpp11_value_init2.a << endl;

  bar bar_default_init;
  bar bar_value_init = bar();
  bar bar_cpp11_value_init{};
  bar bar_cpp11_value_init2 = bar{};
  std::cout << "bar_default_init: " << bar_default_init.b
            << " bar_value_init: " << bar_value_init.b
            << " bar_cpp11_value_init: " << bar_cpp11_value_init.b
            << " bar_cpp11_value_init2: " << bar_cpp11_value_init2.b << endl;
}
```
