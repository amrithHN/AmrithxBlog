---
author: amrithmmh
category:
  - uncategorized
date: "2022-06-30T10:34:02+00:00"
guid: https://armphibian.wordpress.com/?p=192
tag:
  - atmega328p
  - avr
  - embedded-c
title: '[4] AVR C PWM (fast PWM, phase correct PWM  )'
url: /2022/06/30/4-avr-c-pwm-fast-pwm-phase-correct-pwm-and-frequency-phase-correct-pwm/

---
I found this wonderful [article](https://maxembedded.com/2011/08/avr-timers-pwm-mode-part-i/) and [another](https://wolles-elektronikkiste.de/en/timer-and-pwm-part-2-16-bit-timer1) one that explains the theory of PWM and the waveform and where it's used. Since I don't think I can improve on that I will leave it there for reference.

Summary of the above article \[ for lazy people like me ;-) \] :

1. As the title suggests, there are three PWM modes: Fast PWM, phase correct PWM lastly frequency and phase correct PWM
1. Using a sawtooth and TOP, MAX values pulse width gets manipulated
1. It is used to control DC motor, servo motor etc

### FAST PWM

Use any of the three timers for this purpose for hardware PWM generation . (software generation is another topic to be covered separately)

I am using timer 1

```
#include <avr/io.h>
#include <avr/interrupt.h>

//selecting OC1A PB1 --- Arduino digital pin 9

void init_pwm(){

 TCCR1A |= (1<<COM1A1)|(1<<WGM10)|(1<<WGM11);	//Clear OC1A/OC1B on Compare Match (Set output to low level). non inverting
 TCCR1B |=(1<< WGM12);      //mode also use Fast PWM 10bit mode
 OCR1A = 0x1FF; //50 percent duty cycle
 TIMSK1|=(1<<TOIE1);
 sei();
//no prescaling value CS10=1
  TCCR1B|=(1<<CS10);

}

int main(void){
	DDRB|=(1<<PB5); //led
	DDRB|=(1<<PB1); //timer1 pb1
	init_pwm();
	while(1);
	return 0;
}

ISR(TIMER1_OVF_vect){
	PORTB^=(1<<PB5);

}
```

![](/wp-content/uploads/2022/06/screenshot-from-2022-06-27-14-38-29.png?w=1024)50 percent duty cycle fast PWM

**Phase correct PWM**

inorder to demonstrate phase correct PWM two PWM signals are generated

```
#include <avr/io.h>
#include <avr/interrupt.h>

//selecting OC1A PB1 --- Arduino digital pin 9

void init_pwm(){

 TCCR1A |= (1<<COM1A1)|(1<<COM1B1)|(1<<WGM10)|(1<<WGM11);	//Clear OC1A/OC1B on Compare Match (Set output to low level). non inverting
 //TCCR1B |=(1<< WGM12);      //mode also use Fast PWM 10bit mode

 OCR1A = 0x1FF; //50 percent duty cycle
 OCR1B = 0x342;
 TIMSK1|=(1<<TOIE1);

 sei();
//no prescaling value CS10=1
  TCCR1B|=(1<<CS10);

}

int main(void){
	DDRB|=(1<<PB5); //led
	DDRB|=(1<<PB1); //timer1 pb1
	DDRB|=(1<<PB2);
	init_pwm();
	while(1);
	return 0;
}

ISR(TIMER1_OVF_vect){
	PORTB^=(1<<PB5);

}
```

![](/wp-content/uploads/2022/06/screenshot-from-2022-06-27-15-58-53.png?w=1024)
