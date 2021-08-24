---
layout: post
ads: true
comments: true
published: true
title: C++ Primer 5th Notes
categories:
  - program
tags:
  - C++
---
## Preface
Primary goal of new standard (C++11):
- Make the language more uniform and easier to teach and to learn
- Make the standard libraries easier, safer, and more efficient to use
- Make it easier to write efficient abstractions and libraries

Modern C++ parts:
- The low-level language, much of which is inherited from C
- More advanced language features that allow us to define our own types and to organize large-scale programs and systems
- The standard library, which uses these advanced features to provide useful data structures and algorithms

Approach
- Top down: use high-level feature and hide low level details

Icon:
- C++11: feature available on c++11
- A person studying a book: Everyone should read and understand these sections
- Stack of book: can be skipped or skimmed on a first reading, just to have an idea about capability exists, revisit when need to use
- Magnifying-glass: particularly tricky concepts, pay more attention for thorought understand

### Chapter 1: Getting Started

Key concept: data type
```
A type defines both the contents of a data element and the operations that are possible on those data.
```

Standard library: The C++ language does not define any statements to do input or output (IO). Instead, C++ includes an extensive **standard library** that provides IO (and many other facilities).

Standard IO: Predefined IO objects: cin, cout, cerr, clog

Expression: The smallest unit of computation. An expression consists of one or
more *operands* and usually one or more *operators*. Expressions are evaluated to
produce a result.

Literal:
- Number literal: Sequence of zero or more numerical characters (123, 456)
- String literal: Sequence of zero or more characters enclosed in double quotes ("a string literal")

Manipulator: Objects, such as std::endl, that when read or written “manipulates” the stream itself.

Buffer: A region of storage used to hold data. IO facilities often store input (or
output) in a buffer and read or write the buffer independently from actions in the
program.

Output(<<)/Input(>>) operator: output/read data to/from left-hand operand and return that operand.

std::end manipulator: writing *std::endl* has the effect of *ending* the current line and *flushing* the buffer associated with that device.

std::cin: can be used as test condition on (for-loop, while-loop, if statement).

Key Concept: Indentation and Formatting of C++ Programs
```
Always make indentation and formating code for readablity and matainance
```

Key Concept: Classes Define Behavior
- To use a class, we need not care about how it is implemented. Instead, what we need to know is what operations objects of that type can perform
- It is the class author of a class, who determines all the operations that can be used on objects of the class type, need to know how it is implemented

## Part I: The Basics

Most fundamental of common features of a programming language
- Built-in types such as integers, characters, and so forth
- Variables, which let us give names to the objects we use
- Expressions and statements to manipulate values of these types
- Control structures, such as if or while, that allow us to conditionally or repeatedly execute a set of actions
- Functions that let us define callable units of computation
- Facilities which lets us define our own type, data structure, operations

At its core, C++ is a simple language, it provides a set of built-in types, operators to manipulate those types, and a small set of statements for program flow control. Its expressive power arises from its support for mechanisms that allow the programmer to define new data type (class support) and rich of standard libraries.

C++ is a *statically typed* language: type checking is done at compile time.

The first step in mastering C++ is learning the basics of the language and library.

### Chapter 2: Variables and Basic Types

C++ primitive types = arithmatic types + special type named *void*. Arithmatic type = integral types + floating-point type.

The size of, that is, the number of bits in the arithmetic types varies across
machines. The standard *only guarantees minimum sizes* and compilers are allowed to use larger sizes for these types.
 
Unlike the other integer types, there are three distinct basic character types: *char*, *signed char*, and *unsigned char*. In particular, *char* is not the same type as *signed char*. Although there are three character types, there are only two representations: *signed* and *unsigned*. The (plain) *char* type uses one of these representations, but which one is depends on the compiler.

Advice: which type to use
- Use an *unsigned type* when you know that the values cannot be negative.
- Use *int* for integer arithmetic. *short* is usually too small and, in practice, *long* often has the same size as *int*. If your data values are larger than the minimum guaranteed size of an *int*, then use *long long*.
- Do not use plain *char* or *bool* in arithmetic expressions. Use them only to hold characters or truth values. Computations using *char* are especially problematic because *char* is signed on some machines and unsigned on others. If you need a tiny integer, explicitly specify either *signed char* or *unsigned char*.
- Use *double* for floating-point computations; *float* usually does not have enough precision, and the cost of double-precision calculations versus single-precision is negligible. In fact, on some machines, double-precision operations are faster than single. The precision offered by *long double* usually is unnecessary and often entails considerable run-time cost.

Type Conversions
- Type conversion is one of operations which depend on data type
- Happen automatically when we use an object of one type where an object of another type is expected
- Example on arithmetic type assigment
  - *non-bool* to *bool*: *false* if value is 0 and *true* otherwise
  - *bool* to *non-bool*: 0 if the bool is *false*, 1 if the bool is *true*
  - *float* to *integer*: float value is truncated to part before decimal point
  - *integer* to *float*: fractional part is zero. Precision may be lost if the integer has more bits than the floating-point object can accommodate
  - *out-of-range* to *unsigned*: the result is the remainder of the value *modulo* the number of values the target type can hold (EX: an 8-bit unsigned char can hold values from 0 through 255, inclusive. If we assign a value outside this range, the compiler assigns the remainder of that value modulo 256. Therefore, assigning –1 to an 8-bit unsigned char gives that object the value 255)
  - If we assign an out-of-range value to an object of signed type, the result is **undefined**


Mixed Unsigned vs Signed
- Signed values are automatically converted to unsigned using *out-of-range* rule above

```c++
// Example1
unsigned u = 10;
int i = -42;
std::cout << i + i << std::endl;  // prints -84
std::cout << u + i << std::endl;  // if 32-bit ints, prints 4294967264

// Example 2
unsigned u1 = 42, u2 = 10;
std::cout << u1 - u2 << std::endl; // ok: result is 32
std::cout << u2 - u1 << std::endl; // ok: but the result will wrap around

// Example 3
// WRONG: u can never be less than 0; the condition will always succeed
for (unsigned u = 10; u >= 0; --u)
  std::cout << u << std::endl;
```

Advice: Don’t Mix Signed and Unsigned Types

#### Literal
- Character and character string literal
- Integer literal
- Floating-point literal
- Bool and pointer literal

#### Variable
- *Object*: is a region of memory that has a type
- Initialization is not assigment

List initialization
```c
int units_sold = 0;
int units_sold(0);

// list initialization
int units_sold = {0};
int units_sold{0};

// C++11: The compiler will not let us list initialize variables of built-in type 
// if the initializer might lead to the loss of information
long double ld = 3.1415926536;
int a{ld}, b = {ld}; // error: narrowing conversion required
int c(ld), d = ld;   // ok: but value will be truncated
```

Default initialization (variable without initializer)
- build-in type
  - global is initialize to zero
  - local variable has undefined value
- class type: have a value that is defined by the class

Declaration vs definition
- Variables must be defined exactly once but can be declared many times

Identifiers

Scope of a name
- global vs block scope
- Advice: Define Variables Where You First Use Them
- nested scope (inner vs outer scope)

#### Compound Types
A *compound type* is a type that is defined in terms of another type. C++ has
several *compound types*, two of which are *references* and *pointers*

- *declaration*: is a *base type* followed by a list of *declarators*
- *base type*: type specifier, possibly qualified by const, that precedes the declarators in a declaration. The base type provides the common type on which the declarators in a declaration can build
- *declarator*: the part of a declaration that includes the name being defined and an optional type modifier

References:
- References must be initialized. Once initialized, a reference remains bound to its initial object. There is no way to rebind a reference to refer to a different object.
- References are not objects, we may not define a reference to a reference

```c++
int &refVal = ival;  // refVal refers to (is another name for) ival
// ok: refVal3 is bound to the object to which refVal is bound, i.e., to ival
int &refVal3 = refVal;
// initializes i from the value in the object to which refVal is bound
int i = refVal; // ok: initializes i to the same value as ival
```
- A reference may be bound only to an object, not to a literal or to the result of a more general expression
- Type of a reference and the object to which the reference refers must match exactly (with two exceptions: reference to *const* can be initialized from any expression that can be converted to the type of the reference)

Pointer
- A pointer is an object in its own right
- Address-of operator (the **&** operator): return address of an object
- Because references are not objects, they don’t have addresses. Hence, we may not define a pointer to a reference
- The types of the pointer and the object to which it points must match (with two exceptions: pointer to const)
- Dereference operator (the **\*** operator): to access object which a pointer points to
- null pointer:
  - nullptr: a literal
  - NULL: a macro/preprocessor variable

```c++
int *p1 = nullptr; // equivalent to int *p1 = 0;
int *p2 = 0;       // directly initializes p2 from the literal constant 0
// must #include cstdlib
int *p3 = NULL;    // equivalent to int *p3 = 0;
```

- Advice: Initialize all Pointers
  - Define a pointer only after the object to which it should point has been defined
  - If there is no object to bind to a pointer, then initialize the pointer to nullptr or zero
  - *void \** pointer: use to deal with memory as memory, rather than using the pointer to access the object stored in that memory

Compound Type Declaration
- *declaration*: is a *base type* followed by a list of *declarators*
- *base type*: type specifier, possibly qualified by const, that precedes the declarators in a declaration. The base type provides the common type on which the declarators in a declaration can build
- *declarator*: the part of a declaration that includes the name being defined and an optional type modifier
- Type modifiers (\* or &) modify the type of variable not the *base type*

```c++
int* p1, p2; 
// p1 is a pointer to int; p2 is an int
// because the * modifies only type of p1 and the base type still is int not int* 
```

Pointer to Pointer

Reference to pointer
```c++
int i = 42;
int *p;        // p is a pointer to int
int *&r = p;   // r is a reference to the pointer p
r = &i; //  r refers to a pointer; assigning &i to r makes p point to i
*r = 0; //  dereferencing r yields i, the object to which p points; changes i to 0
```

#### *const* Qualifier
- By default, *const* objects are local to a file
- To share a const object among multiple files, you must define the variable as *extern*
- Reference/pointer to *const* restricts only what we can do through that reference/pointer but says nothing about whether the underlying object itself is *const*
- *const* pointer (should read right to left)

```c++
const double *const pip = &pi;
// pip is a const pointer which point to const double
```

Top-level/low-level *const*
  - Top-level const: The const that specifies that an object may not be changed
  - Low-level const: Low-level const appears in the base type of compound types

  ```c++
  int i = 0;
  int *const p1 = &i;  // we can't change the value of p1; const is top-level
  const int ci = 42;   // we cannot change ci; const is top-level
  const int *p2 = &ci; // we can change p2; const is low-level
  const int *const p3 = p2; // right-most const is top-level, left-most is not
  const int &r = ci;  // const in reference types is always low-level
  ```
  - When we copy an object, top-level consts are ignored but low-level const is never ignored. both objects must have the same low-level const qualification or there must be a conversion between the types of the two objects

*constexpr* and Constant Expressions (c++11)
- To ask the compiler to verify that a variable is a constant expression

Literal Types
- The types we can use in a *constexpr*

Pointers and constexpr
- When we define a pointer in a *constexpr* declaration, the *constexpr* specifier applies to the pointer, not the type to which the pointer points to

```c++
const int *p = nullptr;     // p is a pointer to a const int
constexpr int *q = nullptr; // q is a const pointer to int
```
- This is because, *constexpr* imposes a *top-level const* on the objects it defines

#### Dealing With Types
Type Aliases
```c++
typedef double wages;   // wages is a synonym for double
typedef wages base, *p; // base is a synonym for double, p for double*
```
- The keyword *typedef* may appear as part of the *base type* of a declaration. Declarations that include typedef define type aliases rather than variables
- C++11 type alias

```c++
using SI = Sales_item;  // SI is a synonym for Sales_item
```

Pointers, const , and Type Aliases
```c++
typedef char *pstring;
const pstring cstr = 0; // cstr is a constant pointer to char because pstring is base type
const pstring *ps;      // ps is a pointer to a constant pointer to char
```
- It is incorrect to interpret a declaration that uses a type alias by conceptually replacing the alias with its corresponding type

The *auto* Type Specifier (C++11)
- let the compiler figure out the type
- variable that uses auto as its type specifier must have an initializer

```c++
auto item = val1 + val2; // item initialized to the result of val1 + val2

auto i = 0, *p = &i;      // ok: i is int and p is a pointer to int
auto sz = 0, pi = 3.14;   // error: inconsistent types for sz and pi, because a declaration can involve only a single base type
```

Compound Types, const , and auto
- when use a reference as an initializer, the initializer is the corresponding object
- *auto* ignores *top-level consts* and keep *low-level consts*

```c++
int i = 0, &r = i;
auto a = r;  // a is an int (r is an alias for i, which has type int)

const int ci = i, &cr = ci;
auto b = ci;  // b is an int (top-level const in ci is dropped)
auto c = cr;  // c is an int (cr is an alias for ci whose const is top-level)
auto d = &i;  // d is an int*(& of an int object is int*)
auto e = &ci; // e is const int*(& of a const object is low-level const)
```

The decltype Type Specifier (C++11)
- Define a variable with a type that the compiler deduces from an expression
- When deduce type from function, the compiler gives the same type as the type that would be returned if we were to call the function
- The way decltype handles top-level const and references differs subtly from the way *auto* does. When deduce type from a variable, decltype returns the type of that variable, including top-level const and references
- decltype is the only context in which a variable defined as a reference is not treated as a synonym for the object to which it refers

```c++
decltype(f()) sum = x; // sum has whatever type f returns

const int ci = 0, &cj = ci;
decltype(ci) x = 0; // x has type const int
decltype(cj) y = x; // y has type const int& and is bound to x
decltype(cj) z;     // error: z is a reference and must be initialized
```

decltype and References
- *decltype* returns a *reference type* for *expressions* that yield objects that can stand on the left-hand side of the assignment
- *decltype* returns a reference when apply to dereference operator

```c++
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;  // ok: addition yields an int; b is an (uninitialized) int
decltype(*p) c;     // error: c is int& and must be initialized
```
- Deduction done by *decltype* depends on the *form of its given expression*
- decltype(( variable )) (note, double parentheses) is always a reference type, but decltype( variable ) is a reference type only if variable is a reference

```c++
// decltype of a parenthesized variable is always a reference
decltype((i)) d;    // error: d is int& and must be initialized
decltype(i) e;      // ok: e is an (uninitialized) int
```

#### Defining Our Own Data Structures
```c++
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```
- The close curly that ends the class body must be followed by a semicolon  because we can define variables after the class body
- *in-class initializer* (C++11): members without an initializer are default initialized
  - *In-class initializers* are restricted as to the form we can use: They must either be enclosed inside curly braces (*list initializer*) or follow an = sign. We may not specify an in-class initializer inside parentheses
