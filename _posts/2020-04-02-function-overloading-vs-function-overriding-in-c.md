---
layout: post
ads: true
comments: true
published: true
title: Function Overloading vs Function Overriding in C++
categories:
  - program
tags:
  - C++
  - C++ Polymorphism
---
#### Ref
- https://www.geeksforgeeks.org/function-overloading-vs-function-overriding-in-cpp/


#### Example Cpp code

```cpp
// CPP program to illustrate 
// Function Overriding 
#include<iostream> 
using namespace std; 

class BaseClass 
{ 
public: 
  virtual void Display() 
  { 
    cout << "\nThis is Display() method"
        " of BaseClass"; 
  } 
  void Show() 
  { 
    cout << "\nThis is Show() method "
      "of BaseClass"; 
  } 
}; 

class DerivedClass : public BaseClass 
{ 
public: 
  // Overriding method - new working of 
  // base class's display method 
  void Display() 
  { 
    cout << "\nThis is Display() method"
      " of DerivedClass"; 
  } 
}; 

// Driver code 
int main() 
{ 
  DerivedClass dr; 
  BaseClass &bs = dr; 
  bs.Display(); 
  dr.Show(); 
} 
```


#### Class methods are just like others functions, that is they are just some code located somewhere in `.text` (.code) section. Except that a class method will receive as its first argument the famous `this` pointer (passed via `rdi` register in this case). Also, you can see name mangling in actions here (ex: `_ZN9BaseClass4ShowEv, _ZN12DerivedClass7DisplayEv`)

```asm
.text:0000000000000A6C ; =============== S U B R O U T I N E =======================================
.text:0000000000000A6C
.text:0000000000000A6C ; Attributes: static bp-based frame
.text:0000000000000A6C
.text:0000000000000A6C ; void __cdecl BaseClass::Show(BaseClass *const this)
.text:0000000000000A6C                 public _ZN9BaseClass4ShowEv ; weak
.text:0000000000000A6C _ZN9BaseClass4ShowEv proc near          ; CODE XREF: main+44↑p
.text:0000000000000A6C
.text:0000000000000A6C this            = qword ptr -8
.text:0000000000000A6C
.text:0000000000000A6C ; __unwind {
.text:0000000000000A6C                 push    rbp
.text:0000000000000A6D                 mov     rbp, rsp
.text:0000000000000A70                 sub     rsp, 10h
.text:0000000000000A74                 mov     [rbp+this], rdi ; rdi = the *this* pointer
.text:0000000000000A78                 lea     rsi, aThisIsShowMeth ; "\nThis is Show() method of BaseClass"
.text:0000000000000A7F                 lea     rdi, _ZSt4cout@@GLIBCXX_3_4
.text:0000000000000A86                 call    __ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc 
.text:0000000000000A8B                 nop
.text:0000000000000A8C                 leave
.text:0000000000000A8D                 retn
.text:0000000000000A8D ; } // starts at A6C
.text:0000000000000A8D _ZN9BaseClass4ShowEv endp
.text:0000000000000A8D
.text:0000000000000A8E
.text:0000000000000A8E ; =============== S U B R O U T I N E =======================================
.text:0000000000000A8E
.text:0000000000000A8E ; Attributes: static bp-based frame
.text:0000000000000A8E
.text:0000000000000A8E ; void __cdecl DerivedClass::Display(DerivedClass *const this)
.text:0000000000000A8E                 public _ZN12DerivedClass7DisplayEv ; weak
.text:0000000000000A8E _ZN12DerivedClass7DisplayEv proc near   ; DATA XREF: .data.rel.ro:off_201D68↓o
.text:0000000000000A8E
.text:0000000000000A8E this            = qword ptr -8
.text:0000000000000A8E
.text:0000000000000A8E ; __unwind {
.text:0000000000000A8E                 push    rbp
.text:0000000000000A8F                 mov     rbp, rsp
.text:0000000000000A92                 sub     rsp, 10h
.text:0000000000000A96                 mov     [rbp+this], rdi
.text:0000000000000A9A                 lea     rsi, aThisIsDisplayM ; "\nThis is Display() method of DerivedCl"...
.text:0000000000000AA1                 lea     rdi, _ZSt4cout@@GLIBCXX_3_4
.text:0000000000000AA8                 call    __ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc 
.text:0000000000000AAD                 nop
.text:0000000000000AAE                 leave
.text:0000000000000AAF                 retn
.text:0000000000000AAF ; } // starts at A8E
.text:0000000000000AAF _ZN12DerivedClass7DisplayEv endp
.text:0000000000000AAF
```


#### `Vtable` of a class is just array of function pointers to its virtual functions which located somewhere in read-only data section (`.data.rel.ro`)

```asm
.data.rel.ro:0000000000201D58 ; `vtable for'DerivedClass
.data.rel.ro:0000000000201D58 _ZTV12DerivedClass dq 0                 ; offset to this
.data.rel.ro:0000000000201D60                 dq offset _ZTI12DerivedClass ; `typeinfo for'DerivedClass
.data.rel.ro:0000000000201D68 off_201D68      dq offset _ZN12DerivedClass7DisplayEv
.data.rel.ro:0000000000201D68                                         ; DATA XREF: main+17↑o
.data.rel.ro:0000000000201D68                                         ; DerivedClass::Display(void)
```


#### This is how compiler setup and call correct function in case of `polymorphism`

```asm
.text:00000000000009AA ; =============== S U B R O U T I N E =======================================
.text:00000000000009AA
.text:00000000000009AA ; Attributes: bp-based frame
.text:00000000000009AA
.text:00000000000009AA ; int __cdecl main(int argc, const char **argv, const char **envp)
.text:00000000000009AA                 public main
.text:00000000000009AA main            proc near               ; DATA XREF: _start+1D↑o
.text:00000000000009AA
.text:00000000000009AA dr              = DerivedClass ptr -18h
.text:00000000000009AA bs              = qword ptr -10h
.text:00000000000009AA var_8           = qword ptr -8
.text:00000000000009AA
.text:00000000000009AA ; __unwind {
.text:00000000000009AA                 push    rbp
.text:00000000000009AB                 mov     rbp, rsp
.text:00000000000009AE                 sub     rsp, 20h
.text:00000000000009B2                 mov     rax, fs:28h
.text:00000000000009BB                 mov     [rbp+var_8], rax
.text:00000000000009BF                 xor     eax, eax
.text:00000000000009C1                 lea     rax, off_201D68 ; vtable for *DerivedClass*
.text:00000000000009C8                 mov     [rbp+dr.baseclass_0._vptr_BaseClass], rax ; vptr of *ds* = off_201D68
.text:00000000000009CC                 lea     rax, [rbp+dr]   ; rax = addr of *dr*
.text:00000000000009D0                 mov     [rbp+bs], rax   ; vptr of *bs* = vptr of *ds*
.text:00000000000009D4                 mov     rax, [rbp+bs]   ; rax = vptr of *bs*
.text:00000000000009D8                 mov     rax, [rax]      ; rax = addr of vtable of *bs*
.text:00000000000009DB                 mov     rax, [rax]      ; rax = first func in vtable of *bs*
.text:00000000000009DE                 mov     rdx, [rbp+bs]   ; rdx = addr of *bs/dr*
.text:00000000000009E2                 mov     rdi, rdx        ; rdi = addr of *bs/dr*
.text:00000000000009E5                 call    rax             ; call *Display* function with first arg in *rdi* (this pointer)
.text:00000000000009E7                 lea     rax, [rbp+dr]
.text:00000000000009EB                 mov     rdi, rax        ; this
.text:00000000000009EE                 call    _ZN9BaseClass4ShowEv ; BaseClass::Show(void)
.text:00000000000009F3                 mov     eax, 0
.text:00000000000009F8                 mov     rcx, [rbp+var_8]
.text:00000000000009FC                 xor     rcx, fs:28h
.text:0000000000000A05                 jz      short locret_A0C
.text:0000000000000A07                 call    ___stack_chk_fail
.text:0000000000000A0C ; ---------------------------------------------------------------------------
.text:0000000000000A0C
.text:0000000000000A0C locret_A0C:                             ; CODE XREF: main+5B↑j
.text:0000000000000A0C                 leave
.text:0000000000000A0D                 retn
.text:0000000000000A0D ; } // starts at 9AA
.text:0000000000000A0D main            endp
```

Pseudo code of the `main` function above

```C
int __cdecl main(int argc, const char **argv, const char **envp)
{
  DerivedClass dr; // [rsp+8h] [rbp-18h]
  BaseClass *bs; // [rsp+10h] [rbp-10h]
  unsigned __int64 v6; // [rsp+18h] [rbp-8h]

  v6 = __readfsqword(0x28u);
  dr._vptr_BaseClass = (int (**)(...))&off_201D68;
  bs = (BaseClass *)&dr;
  DerivedClass::Display(&dr);
  BaseClass::Show(&dr.0);
  return 0;
}
```
