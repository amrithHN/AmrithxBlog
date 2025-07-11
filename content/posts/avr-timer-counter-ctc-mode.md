---
author: amrithmmh
cover:
  alt: index
  image: /wp-content/uploads/2022/06/index.png
date: "2022-06-26T10:53:47+00:00"
guid: https://armphibian.wordpress.com/?p=166
tag:
  - atmega328p
  - avr
  - embedded-c
title: '[2] AVR timer/ counter -- CTC mode'
url: /2022/06/26/2-avr-timer-counter-ctc-mode/

---
There is a TCNTn called timer/Counter Register for each of the timers in AVR. For example, In Atmega32 we have TCNT0, TCNT1 and TCNT2.  The TCNTn is basically a counter. It counts up with each pulse. It contains zero when reset. We can read or load any value in the TCNTn register.Each of the timer has TOV called Time Overflow. The TOVn flag is set when the timer overflows, where ‘n’ is the any number of a timer.

These timers have also Timer/Counter Control Register. It is denoted as TCCRn. It is used for setting up the modes of the timer.Another register of the timer is OCRn which is named as “Output Compare register”. It is used as Compare match register. Here the contents of OCRn are compared with the contents of TCNTn.

**refer** [this](https://www.arxterra.com/9-atmega328p-timers/) **link to know more in detail** or [this](https://wolles-elektronikkiste.de/en/timer-and-pwm-part-1-8-bit-timer0-2)

Atmega328p is equipped  with timer0, timer1, timer2; out of which two are 8-bits and one is 16-bit. Maximum number of clock ticks that a timer can count depends on the size of the register.

Timer 0 and timer 2 use two different 8-bit registers, whereas timer 1 uses a 16-bit register.

An 8-bit register can count up to 2^8 = 256(0 to 255). Similarly 16-bit register can count up to 2^16 = 65536(0 to 65535). With the available resources, I can generate an interrupt at every (65536/clock freq) 65536/1,60,00,000 = 4.0959375ms.

An excellent video I found explaining the whole concept!

{{< youtube OReeWLpipmk >}}&ab\_channel=JoelCastillo

```
#include <avr/io.h>
#include <avr/interrupt.h>
//using timer 0 for this example

void timer_config(){
OCR0A= 194; //40 hz signal for N=1024 prescalar
TCCR0A =(1<<WGM01);
TIMSK0=(1<<OCIE0A); // Timer/Counter0 Compare Match A interrupt is enabled
TCCR0B = (1<<CS02) | (1<<CS00) ; //CTC mode
sei();

}

void led_config(){
	//PB5 led as output
	DDRB|=(1<<PB5); //set as output

}

int main(void){
led_config();
timer_config();

while(1){
	//do nothing
}
}

ISR(TIMER0_COMPA_vect){
PORTB^=(1<<PB5);
}

```

Above code when flashed generates 40hz (25ms,50% PWM wave) on PB5 which is also connected to LED, hence it shines a little brighter.Using Sigrok and Salaelogic analyser I was able to capture the signal. below is the screenshot for the same

![](/wp-content/uploads/2022/06/screenshot-from-2022-06-26-16-12-33.png?w=1024)pulseview+sigrok in ubuntu with salaelogic analyser
