---
author: amrithmmh
category:
  - uncategorized
cover:
  alt: index
  image: /wp-content/uploads/2022/06/index.png
date: "2022-06-25T08:11:32+00:00"
guid: https://armphibian.wordpress.com/?p=148
tag:
  - atmega328p
  - avr
  - embedded-c
title: AVR GCC AVRdude flashing using CLI (makefile example included)
url: /2022/06/25/avr-gcc-avrdude-flashing-using-cli-makefile-example-included/

---
simple hello world program for atmega328\*:

```
#include <avr/io.h>
#include <util/delay.h>

/*
Tells compiler the cpu speed to use for delays
need to define it before importing util/delay.h
otherwise define it with -DF_CPU=16000000UL during compilation
*/
#ifndef F_CPU
#define F_CPU 16000000UL
#endif
/* PB5 has value of 5 */
#define LED       PB5
#define SLEEP_MS  500

int main(void) {
    /* initializing PB5 which is connected to port 13 of uno as output*/
    DDRB |= (1<<LED);

    while (1) {
        /* toggling the PB5 to alternate between on-off state*/
        PORTB ^= (1<<LED);
        _delay_ms(SLEEP_MS);
    }
}
```

compiling using avr-gcc cli: [avr gcc flags](https://www.nongnu.org/avr-libc/user-manual/group__demo__project.html)

```
avr-gcc -g -Os -mmcu=atmega328 -c demo.c
avr-gcc -g -mmcu=atmega328 -o demo.elf demo.o
avr-objcopy -O ihex -R .eeprom demo.elf demo.hex
```

flashing .hex file: **usbasp** is the programmer used!

```
avrdude -c usbasp -p atmega328 -U flash:w:demo.hex
```

To automate the whole process lets write a make file! example : [avrfreaks](https://www.avrfreaks.net/sites/default/files/Makefile.txt)

**makefile** (updated on jul 4 2022)

```
FILENAME   = blink
PROGRAMMER = usbasp
DEVICE     = atmega328
COMMAND    = avr-gcc -Wall -g -Os -mmcu=${DEVICE} -D_CPU=16000000

default: compile upload clean

compile:
	${COMMAND} -c ${FILENAME}.c -o ${FILENAME}.o
	${COMMAND} -o ${FILENAME}.elf ${FILENAME}.o
	avr-objcopy -R .eeprom -O ihex ${FILENAME}.elf  ${FILENAME}.hex
	avr-size --format=avr --mcu=${DEVICE} ${FILENAME}.elf

upload:
	avrdude -v -p ${DEVICE} -c ${PROGRAMMER} -P ${PORT} -b ${BAUD} -U flash:w:${FILENAME}.hex:i

clean:
	rm *.o
	rm *.elf
	rm *.hex

```
