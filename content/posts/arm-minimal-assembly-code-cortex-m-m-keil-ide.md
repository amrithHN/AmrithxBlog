---
author: amrithmmh
category:
  - stm32
cover:
  alt: IMG_20221003_093302
  image: /wp-content/uploads/2022/10/img_20221003_093302.jpg
date: "2022-10-03T13:44:53+00:00"
guid: https://armphibian.wordpress.com/?p=372
tag:
  - architectre
  - arm
  - asm
  - assembly
  - cortex-m3
  - thumb
  - thumb2
title: '[1] ARM Minimal Assembly code (cortex m3/m4 + keil IDE)'
url: /2022/10/03/1-arm-minimal-assembly-code-coretx-m3-m4-keil-ide/

---
ARM processors have supported two different instruction sets: the **ARM instructions** that are **32 bits** and **Thumb instructions** that are **16 bits**. Thumb2 instructuction was introduced later which are 32bit instruction and the processor doesnt have to switch between modes to perform instructions. **ARM is faster as it requires less clock cycles** for execution **whereas Thumb instruction require high code density** (ex ARM perform an operation with 3 instructions whereas thumb can do the same with 2 thus saving code space but reducing operating speed since it is 16bit instruction).

![](/wp-content/uploads/2022/10/thumb.png?w=1024)

**Registers**

1. General-purpose registers, R0-R12
   - R0-R7 - low registers (can be used in all instructions)
   - R8-R12 - high registers (cannot be used by thumb instruction)
   - Stack Pointer (SP): R13 is used as SP. banked register with two copies, namely Main Stack Pointer (MSP)and Process Stack Pointer (PSP)
   - Link Register (LR): R14 stores the value of return address when a function is called
   - Program Counter(PC): R15 points to next instruction to be executed
1. Program Status Registers (PSRs):
   - Application Program Status Register (APSR)
   - Interrupt Program Status Register (IPSR)
   - Execution Program Status Register (EPSR)
1. Control register (CONTROL): The control register is used to configure theprivilege level as well as to perform the selection of stack pointer register

Note: Why microprocessors have separate carry and overflow flags? The carry flag represents overflow for unsigned numbers while signed number overflow is indicated by the overflow flag.Signed and unsigned overflows occur independently. There can be four possible out-comes when an arithmetic operation such as addition is performed. (1) no overflow,(2) unsigned overflow only, (3) signed overflow only, and (4) both signed and unsignedoverflows.

**Reset sequence**

![](/wp-content/uploads/2022/10/rest_m.png?w=1024)![](/wp-content/uploads/2022/10/reset_mem.png?w=807)

**Note: The instructions written as part of the reset handler are also sometimes referred to as bootcode.**

An assembly program needs to perform the following sequence of activities to ensure that the user application program is executed properly.

- Define the stack size and reserve appropriate memory space for the stack.
- Define the reset vector.
- Write the reset handler code to perform any system-related initializations and then make a jump to the user application program

Note : Cortex-M processors do not support ARM mode instructions. They only support Thumb-2, which includes a mixture of 16 and 32-bit instructions. There is no way to run ARM instructions on a Cortex-M

```
 PRESERVE8
 THUMB ; Vector Table Mapped to Address 0 at Reset
  ; Linker requires __Vectors to be exported
	AREA RESET, DATA, READONLY
	EXPORT __Vectors

__Vectors 	DCD 0x20008000 ; initial Stack Pointer
			DCD Reset_Handler ; reset vector

 ; space 8
ALIGN

 ; Linker requires Reset_Handler

 AREA MYCODE, CODE, READONLY

 ENTRY
 EXPORT Reset_Handler
Reset_Handler
	MOV R0 , #0 ; Initial value of sum
	MOV R1 , #2 ; First even number
	MOV R2 , #5 ; Counter for the loop iterations
lbegin
	CBZ R2 , lend ; Terminate loop if counter is zero
	ADD R0 , R1 ; Build the sum
	ADD R1 , #2 ; Generate next even number
	SUB R2 , #1 ; Decrement the counter
	B lbegin
lend
	END
```

The above code initialises the stack pointer and upon reset\_handler, some basic add, sub-operations are performed. This is the most minimalistic program. There are other vectors that need to be initialised that have been omitted because we do not need them for this program ;-)

**note: using Keil IDE, be sure to intend code properly else it throws error!**

![](/wp-content/uploads/2022/10/screenshot-2022-10-03-091746.png?w=1024)keil ide in debug mode with the above code

_**remaining opcodes in further posts!**_ **_Have a good day !_**
