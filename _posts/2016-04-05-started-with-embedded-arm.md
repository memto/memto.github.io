---
layout: post
title: 'Started with Embedded ARM'
categories: program
tags: 'linux arm'
---

In this post I ref to FriendlyARM Tiny6410 board. This board is not so popular (since at the time I bought this board, I did not have much experience with embedded ARM to choose a right one, poorly). 

#### Refs:
[arm1176jzfs_r0p7-TechnicalRefManual.pdf]({{ site.baseurl }}/downloads/docs-hidden/arm1176jzfs_r0p7-TechnicalRefManual.pdf) <br>
[S3C6410XH-66.pdf]({{ site.baseurl }}/downloads/docs-hidden/S3C6410XH-66.pdf) <br>
[S3C6410 Datasheet.pdf]({{ site.baseurl }}/downloads/docs-hidden/S3C6410-Datasheet.pdf) <br>
[Tiny6410 User manual.pdf]({{ site.baseurl }}/downloads/docs-hidden/Tiny6410-User-manual.pdf) <br>

#### Sources

#### Arch / CPU / SoC / Board
> FriendlyArm Tiny6410 board </br>
CPU: 533 MHz Samsung S3C6410A ARM1176JZF-S with VFP-Unit and Jazelle (max freq. 667 MHz) </br>
RAM: 256 MB, 32 bit Bus </br>

- Arch: ARMv6Z - define instruction sets and registers (https://en.wikipedia.org/wiki/List_of_ARM_microarchitectures)
- CPU: S3C6410A/ARM1176JZF-S - S3C6410A is a specific processor design base on the core design ARM1176JZF-S, this CPU will load and execute instructions defined instruction sets
- SoC: S3C6410 - contain a CPU and other components
	- Memory: RAM, SRAM, SDRAM...
	- Interface IC (intergrated IC): ADC, DAC, UART, USB, SPI, I2C...
- Board: Tiny6410 - a board of circuit that connect SoC and input/output devices
	- Inputs: Switch, Buttons, Keyboads, Touch, Mic...
	- Outputs: LEDs, LCD, Speaker...

#### ARM core naming (ex: ARM11/76/JZF-S)
- Family Version Number: 11 <br>
**Standard feature (not need include after ARM7TDMI):**
- T/T2: Thumb/Thumb2 instruction support
- D: JTAG debugging
- M: Fast Mutilplier 
- I: Embedded ICE module <br>
**New feature:**
- Memory interface:
	- 26/36/76: MMU
	- 46: MPU
- J: Jazelle DBX Technology
- Z: Trust Zone
- F: Floating point
- S: Synthesizable