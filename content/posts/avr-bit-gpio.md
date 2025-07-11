---
author: amrithmmh
category:
  - uncategorized
cover:
  alt: index
  image: /wp-content/uploads/2022/06/index.png
date: "2022-06-26T06:37:59+00:00"
guid: https://armphibian.wordpress.com/?p=156
tag:
  - atmega328p
  - avr
  - embedded-c
title: '[1] AVR 8 bit  - GPIO'
url: /2022/06/26/1-avr-8-bit-gpio/

---
For GPIO only these 3 registers are important for bit manipulation.

For each port there are three important registers:

- The **Data Direction Register (DDRx)** determines whether the pins operate as inputs or outputs.
- The **port output register (PORTx)** determines the actual value set on each pin when it’s being used as an output.
- The **port input register (PINx)** is used for reading input values.

x=B,C,D : example DDRB,DDRC,DDRD etc , PORTB,PORTC,PORTD etc , PINB,PINC,PIND etc.

Blink.c

```
#include <avr/io.h>

/*
Tells compiler the cpu speed to use for delays
need to define it before importing util/delay.h
otherwise define it with -DF_CPU=16000000UL during compilation
*/
#ifndef F_CPU
#define F_CPU 16000000UL
#endif

#include <util/delay.h>

/* PB5 has value of 5 */
#define LED       PB5
#define SLEEP_MS  5000

// led is connected to PB5

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

Input using push button without any input pullup (debounce not implemented )

```
//atmega328p + switch test without debounce
//pd7 is the input switch
//pb5 is the led

#include <avr/io.h>
#include <util/delay.h>

#ifndef F_CPU
#define F_CPU 16000000UL
#endif

#define LED PB5
#define SW  PD7

int main(void){

	DDRB |= (1<<LED); //output //pb5 set 1 output
	DDRD &= ~(1<<SW); //set 7th bit as input

	while(1){//main infinite loop

	if((PIND & (1<<SW)))
    {
		PORTB ^= (1<<LED); //toggle led
        _delay_ms(3000);// delay for doing blink on pushed

    }
	else
		PORTB &= ~(1<<LED);

	}

	return 0;
}
```

input pullup fo switch write 1 to the pin bit using PORTx=(1<<PIN)

```
//atmega328p + switch test without debounce
//pd7 is the input switch
//pb5 is the led

#include <avr/io.h>
#include <util/delay.h>

#ifndef F_CPU
#define F_CPU 16000000UL
#endif

#define LED PB5
#define SW  PD7

int main(void){

	DDRB |= (1<<LED); //output //pb5 set 1 output
	DDRD &= ~(1<<SW); //set 7th bit as input
        PORTD |=(1<<SW);  //turning on internal pullup

	while(1){//main infinite loop

	if((PIND & (1<<SW))==0)// button pressed
    {  //blink led
		PORTB ^= (1<<LED);
        _delay_ms(3000);

    }
	else
		PORTB &= ~(1<<LED);

	}

	return 0;
}
```
