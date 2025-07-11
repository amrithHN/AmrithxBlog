---
author: amrithmmh
category:
  - uncategorized
date: "2022-10-02T04:00:00+00:00"
guid: https://armphibian.wordpress.com/?p=338
title: AVR Assembly code
url: /2022/10/02/avr-assembly-code/

---
![](/wp-content/uploads/2022/09/avr_assembly.png?w=1024)

**Simple code example for load and add instruction:**

```
.equ sum = 0x30
.org 00
main: ldi r16,0xFA ; hex value copy to r16
ldi r17,$AB ; dollar sign to hex
ldi r18,0b00001111 ; binary
add r16,r17 ;
add r16,r17 result r16
ldi r19,11 ; decimal 11
sts sum,r16 ; save value to location 30h (sum is a pointer)
here: jmp here
```

![](/wp-content/uploads/2022/09/memory_avr.png?w=1024)![](/wp-content/uploads/2022/09/avr_architecture.png?w=1002)

AVR MCUs' are generally fast, they can execute 16MIPS at 16Mhz because of how the busses are designed and also the simplicity of RSIC architecture.

1. Program bus-> has data bus that is 16 bit wide , this enable the cpu to fetch a word at a time

**Input/ output using gpio:**

steps: set DDRx to 0 for input , 1 for output

write PORTx value to turn on or off

```
.include "m328pdef.inc"
.org 00;PB5 led PD7 switch
main: ldi r16,0xFF
out DDRD,r16
here: ldi r17,0xFF
out PORTD,r17
call delay
ldi r17,0x00
out PORTD,r17
call delay
jmp here
delay: ldi r18,0xF
Fouter: ldi r19,0x10
inner: dec r19
brne inner
dec r18
brne outer
ret
```

![](/wp-content/uploads/2022/09/addr.png?w=1024)

```
.INCLUDE "M328PDEF.INC"
.ORG 00;PB5 LED PD7 SWITCH
MAIN: INC R16        ; 1. SINGLE REGISTER IMMEDIATE
ADD R20,R23    ; 2. REGISTER ADDRESSING MODE
LDS R21,0X230  ; 3. DIRECT ADDRESSING MODE
IN R16,PINB    ; 4. IO ADDRESSING MODE
LDI XL,0X30    ; 5. REGISTER INDIRECT ADDRESSING MODE
LDI XH,0X01
LD R18,X
ST X+,R18      ; USING INCREMENT ON X,Y,Z
LDD R4,Y+5    ; 5. REGISTER INDIRECT WITH DISPLACEMENT
; 7.FLASH INDIRECT ADDRESSING
LPM ; R0 -> Z
IJMP ; FLASH DIRECT ADDRESSING MODE , MOVES TO ADDRESS CONTAINED IN Z
```

_`NOTE`:Stack pointer initial value is 0x0000, meaning it would point to register R0 (which adress is 0x0000) if not initialized. You would not want that, as you use R0 and other register to perform operations. That is why you want to set the stack to some other memory area, specifically to the Internal SRAM (a general purpose RAM area). So initialise stack pointer to a safe value before using function calls, and interrupt service . it is better to init stack before any program._

NOTE: The AVR stack pointer is implemented as two 8-bit registers in the I/O space

```
.MACRO INITSTACK
LDI R20,HIGH(RAMEND)
OUT SPH,R20	;SPH AND SPL ARE IO MEMORY SPACE
LDI R20,LOW(RAMEND)
OUT SPL,R20
.ENDMACRO
```

**Timers/counters programming**

**8 bit timer in normal mode (stack pointer initialised before main program code)**

```
.include "m328pdef.inc"
.MACRO INITSTACK
	LDI R20,HIGH(RAMEND)
	OUT SPH,R20
	LDI R20,LOW(RAMEND)
	OUT SPL,R20
.ENDMACRO
;timer 0 programming
MAIN:	INITSTACK
		CBI DDRB,5  ;PB5 AS OUTPUT
AGAIN:	CBI PORTB,5	; CLEAR PB5
		CALL DELAY
		SBI PORTB,5
		CALL DELAY
		JMP AGAIN

DELAY:	LDI R16,0X00
		OUT TCNT0,R16
		LDI R17,0X00
		OUT TCCR0A,R17
		LDI R18,0X01
		OUT TCCR0B,R18 ; START TIMER
HERE:   IN R20,TIFR0
		SBRS R20,0 ; R20(0) == 1 OR 0  CHECK BITS WITH TIFR0 POSITION  Skip if Bit in Register is Set
		RJMP HERE
		LDI R20,0X0
		OUT TCCR0B,R20
		LDI R20,(1<<TOV0)
		OUT TIFR0,R20  ;TOV0 is cleared by writing a logic one to the flag
		RET
```

**interrupt programming**

**timer 0 in interrupt mode**

![](/wp-content/uploads/2022/09/atmega_vector-table.png?w=988)

note:timer 0 overflow vector should be placed in 0x0020

```
.include "m328pdef.inc"
.MACRO INITSTACK
	LDI R20,HIGH(RAMEND)
	OUT SPH,R20
	LDI R20,LOW(RAMEND)
	OUT SPL,R20
.ENDMACRO
;timer 0 interrupt
.ORG 00
JMP MAIN

.ORG 0X020
JMP TIM_OVF_ISR

.ORG 0X50
TIM_OVF_ISR: IN R18,PORTB
			 LDI R17,(1<<5)
			 EOR R18,R17
			 OUT PORTB,R18
			 RETI

.ORG 0x200
MAIN:	INITSTACK
		LDI R16,0X00
		OUT PORTB,R16
		CBI DDRB,5
		CBI PORTB,5
		;TIMER 0 CONF OVF INTERRUPT ENABLE
		LDI R16,0X00
		OUT TCNT0,R16
		LDI R16,0X01
		OUT TCCR0B,R16   ;START TIMER NO PRESCALAR
		LDI R17,(1<<TOIE0)
		STS TIMSK0,R17 ;ENABLE TIMER OVERFLOW INTERRUPT
		SEI

LOOP:   JMP LOOP

```

REFERENCE : 1. [TIMER APPLICATION NOTE](https://ww1.microchip.com/downloads/en/Appnotes/Atmel-2505-Setup-and-Use-of-AVR-Timers_ApplicationNote_AVR130.pdf)

2\. [AVR registers manual](http://ww1.microchip.com/downloads/en/devicedoc/atmel-0856-avr-instruction-set-manual.pdf)

3\. [atmega328p](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf)
