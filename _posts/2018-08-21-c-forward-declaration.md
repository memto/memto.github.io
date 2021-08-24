---
layout: post
ads: true
comments: true
published: true
title: C++ forward declaration
categories:
  - program
tags:
  - C++
  - C++ forward declaration
---
#### Ref
- [Top 10 C++ header file mistakes and how to fix them](http://www.acodersjourney.com/2016/05/top-10-c-header-file-mistakes-and-how-to-fix-them/)
- [Forward Class Declaration in Cpp](http://jatinganhotra.com/blog/2012/11/25/forward-class-declaration-in-c-plus-plus/)

Example: 
- main.cpp include ChildClassA1.h because it need create an instance of ChildClassA1
- ChildClassA(B) need include BaseClassA(B).h because of inheritence
- BaseClassA.h need include ChildClassB1.h because it need create an instance of ChildClassB1
- BaseClassB.h need include BaseClassA.h because it only need pointer to BaseClassA 

```cpp
// main.cpp
#include "ChildClassA1.h"

main(int argc, char const *argv[])
{
  BaseClassA *classA = new ChildClassA1()
  return 0;
}


// BaseClassA.cpp
#include "BaseClassA.h"

// BaseClassA.h
#ifndef BaseClassA_H
#define BaseClassA_H

#include "ChildClassB1.h"

class BaseClassA
{
private:
  BaseClassB *baseB;
public:
  BaseClassA() { }
  ~BaseClassA() { }
};
#endif

// ChildClassA1.h
#ifndef ChildClassA1_H
#define ChildClassA1_H

#include "BaseClassA.h"

class ChildClassA1 : public BaseClassA
{};
#endif


// BaseClassB.cpp
#include "BaseClassB.h"

// BaseClassB.h
#ifndef BaseClassB_H
#define BaseClassB_H

#include "BaseClassA.h"

class BaseClassB
{
private:
  BaseClassA* baseA;

public:
  BaseClassB(/* args */) {}
  ~BaseClassB() {}
};
#endif

// ChildClassB1.h
#ifndef ChildClassB1_H
#define ChildClassB1_H

#include "BaseClassB.h"

class ChildClassB1 : public BaseClassB
{};
#endif
```

Compile:
```bash
$ g++ main.cpp BaseClassA.cpp BaseClassB.cpp
In file included from ChildClassB1.h:4:0,
                 from BaseClassA.h:3,
                 from ChildClassA1.h:4,
                 from main.cpp:1:
BaseClassB.h:9:3: error: ‘BaseClassA’ does not name a type
   BaseClassA* baseA;
   ^
In file included from ChildClassB1.h:4:0,
                 from BaseClassA.h:3,
                 from BaseClassA.cpp:1:
BaseClassB.h:9:3: error: ‘BaseClassA’ does not name a type
   BaseClassA* baseA;
   ^
In file included from BaseClassA.h:3:0,
                 from BaseClassB.h:4,
                 from BaseClassB.cpp:1:
ChildClassB1.h:7:1: error: expected class-name before ‘{’ token
 {};
 ^
In file included from BaseClassB.h:4:0,
                 from BaseClassB.cpp:1:
BaseClassA.h:8:3: error: ‘BaseClassB’ does not name a type
   BaseClassB *baseB;
```

- First Error:
```bash
In file included from ChildClassB1.h:4:0,
                 from BaseClassA.h:3,
                 from ChildClassA1.h:4,
                 from main.cpp:1:
BaseClassB.h:9:3: error: ‘BaseClassA’ does not name a type
   BaseClassA* baseA;
```

Here main.cpp include ChildClassA1.h which in turn include BaseClassA.h (BaseClassA is not declare yet, but header guard BaseClassA_H is defined). BaseClassA.h then include ChildClassB1.h which in turn include BaseClassB.h; Here, BaseClassB.h include BaseClassA.h but header guard BaseClassA_H is defined so it is skipped. That cause the error: ‘BaseClassA’ does not name a type.

- First fix: We can try to fix this first error using forward declaration of BaseClassA in BaseClassB.h because here BaseClassB.h only needs a **declaration** of BaseClassA **not definition**.

```cpp
#ifndef BaseClassB_H
#define BaseClassB_H

class BaseClassA;

class BaseClassB
{
private:
  BaseClassA* baseA;

public:
  BaseClassB(/* args */) {}
  ~BaseClassB() {}
};
#endif
```

Now we can have a success compilation. But let refator other code also because as above fix we don't need to include whole header in case of forward declaration is enough (this refactor can help to reduce compilation time). Before we do that, let point out some cases when we have to include header files, and when forward declarations is enough:

##### When forward declaration is enough
- declaration of data members as pointer or reference type
- function *declaration* parameters, return type

##### When have to use include:
- declaration of data members as class instance
- function *definition* parameters, return type
- using class as a base class (eg: ChildClassA1.h, ChildClassB1.h)
- create object from class, dereference object pointer, access object methods

##### Apply the rules as above:
- BaseClassA.h only need forward declaration of BaseClassB
- BaseClassA.cpp need include ChildClassB1.h because it will need create instance of ChildClassB1
- BaseClassB.cpp need include BaseClassA.h because it will need dereference and access baseA

```cpp
// BaseClassA.cpp
#include "BaseClassA.h"
#include "ChildClassB1.h"

// BaseClassA.h
#ifndef BaseClassA_H
#define BaseClassA_H

class BaseClassB;

class BaseClassA
{
private:
  BaseClassB *baseB;
public:
  BaseClassA() { }
  ~BaseClassA() { }
};
#endif

// BaseClassB.cpp
#include "BaseClassB.h"
#include "BaseClassA.h"
```
