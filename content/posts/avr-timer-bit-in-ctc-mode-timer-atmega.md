---
author: amrithmmh
category:
  - uncategorized
cover:
  alt: index
  image: /wp-content/uploads/2022/06/index.png
date: "2022-06-27T10:00:00+00:00"
guid: https://armphibian.wordpress.com/?p=186
tag:
  - atmega328p
  - avr
  - embedded-c
title: '[3] AVR timer 16 bit in CTC mode - timer 1  atmega328'
url: /2022/06/27/3-avr-timer-16-bit-in-ctc-mode-timer-1-atmega328/

---
1. set Timer Control register 1 (TCCR1B since WGM12 should be 1 for CTC)
1. load the value to be compared in Overflow Compare register (OCR1A)
1. enable interrupt for OCIE register using Timer1 Mask register (TIMSK1)
1. enable register and set prescalar (1024 in the below example)
1. use ISR

```
#include <avr/io.h>
#include <avr/interrupt.h>
//timer 1 16 bit

void timer_config(){

	 TCCR1B|=(1<<WGM12); //CTC mode
 	 OCR1A=15625;
 	 TIMSK1=(1<<OCIE1A);
	 sei();
	 TCCR1B|=(1<<CS12)|(1<<CS10);//prescalar 1024
	 TCCR1B&=~(1<<CS11);

}

void led_config(){
	//PB5 has led and logic analyser connected
	DDRB|=(1<<PB5);

}

int main(void){

	led_config();
	timer_config();

	while(1){

	}

	return 0;
}

ISR(TIMER1_COMPA_vect){
PORTB^=(1<<PB5);
}
```

![](/wp-content/uploads/2022/06/screenshot-from-2022-06-26-19-48-37.png?w=1024)

use [this](https://eleccelerator.com/avr-timer-calculator/) online calculator for ticks calculations.
