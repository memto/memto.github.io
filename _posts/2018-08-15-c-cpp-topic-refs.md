---
layout: post
ads: true
comments: true
published: true
title: C++11 Topic Refs
categories:
  - program
series:
  - C++11
tags:
  - C++11
  - Rvalue
  - Move semantic
  - Perfect forwarding
---
#### Rvalue/Rvalue reference/Move semantic cpp
- [C++ rvalue references and move semantics for beginners](https://www.internalpointers.com/post/c-rvalue-references-and-move-semantics-beginners)
- [http://kholdstare.github.io/technical/2013/11/23/moves-demystified.html#std-move](http://kholdstare.github.io/technical/2013/11/23/moves-demystified.html)
- [C++ Rvalue References Explained](http://thbecker.net/articles/rvalue_references/section_01.html)

Example
```cpp
/*
file: movesemantic.cpp
brief: move semantic demo
compile: g++ -fno-elide-constructors -g -std=c++11 movesemantic.cpp -o
movesemantic (Note: -fno-elide-constructors which disable copy elision)
*/

#include <iostream>

using namespace std;

#define MOVE_SEMANTIC 1

class Holder
{
public:
  Holder(int size) // Constructor
  {
    m_id = m_object_count++;
    cout << "Constructor " << m_id << endl;

    m_data = new int[size];
    m_size = size;
  }

  ~Holder() // Destructor
  {
    cout << "Destructor " << m_id << endl;

    delete[] m_data;
  }

  Holder(const Holder& other)
  {
    m_data = new int[other.m_size];                          // (1)
    copy(other.m_data, other.m_data + other.m_size, m_data); // (2)
    m_size = other.m_size;

    m_id = m_object_count++;
    cout << m_id << " copy constructor from " << other.m_id << " size "
         << other.m_size << endl;
  }

#if MOVE_SEMANTIC == 1
  Holder(Holder&& other) // <-- rvalue reference in input
  {
    m_data = other.m_data; // (1)
    m_size = other.m_size;
    other.m_data = nullptr; // (2)
    other.m_size = 0;

    m_id = m_object_count++;
    cout << m_id << " move constructor from " << other.m_id << endl;
  }
#endif

  Holder& operator=(const Holder& other)
  {
    if (this == &other)
      return *this;  // (1)
    delete[] m_data; // (2)
    m_data = new int[other.m_size];
    std::copy(other.m_data, other.m_data + other.m_size, m_data);
    m_size = other.m_size;

    m_id = m_object_count++;
    cout << m_id << " assignment operator from " << other.m_id << endl;
    return *this; // (3)
  }

#if MOVE_SEMANTIC == 1
  Holder& operator=(Holder&& other) // <-- rvalue reference in input
  {
    if (this == &other)
      return *this;

    delete[] m_data; // (1)

    m_data = other.m_data; // (2)
    m_size = other.m_size;

    other.m_data = nullptr; // (3)
    other.m_size = 0;

    m_id = m_object_count++;
    cout << m_id << " move assignment operator from " << other.m_id << endl;
    return *this;
  }
#endif

private:
  int* m_data;
  size_t m_size;
  static int m_object_count;
  int m_id;
};

int Holder::m_object_count = 0;

Holder
createHolder(int size)
{
  return Holder(size);
}

main(int argc, char const* argv[])
{
  int var1 = 10;
  int var2 = 20;

  // int&& xx = var2;

  // int& var_lref = var1;
  // int&& var_rref = var1 + var2;
  // var_rref += 30;
  // *(&var_lref) = 30;
  // *(&var_rref) = 10;
  // cout << var_rref << " " << &var_lref << " " << &var1 << " " << var1 << " "
  //      << &var_rref << endl;

  // int& rref_lref = var_rref;
  // rref_lref = 100;
  // cout << var_rref << endl;

  // Holder h1(10000); // regular constructor
  // Holder h2 = h1;   // copy constructor
  // Holder h3(h1);    // copy constructor (alternate syntax)

  // Holder h1(10000); // regular constructor
  // Holder h2(60000); // regular constructor
  // h1 = h2;          // assignment operator

  // Holder h4 = createHolder(100);

  Holder h1(1000); // regular constructor
  // std::move cast lvalue to rvalue that trigger move semantic
  Holder h3(std::move(h1));
  Holder h2(h1);

  return 0;
}
```

#### Perfect forward
- [Perfect forwarding and universal references in C++](https://eli.thegreenplace.net/2014/perfect-forwarding-and-universal-references-in-c)
- [Perfect Forwarding: The Problem](http://thbecker.net/articles/rvalue_references/section_07.html)
