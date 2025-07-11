---
author: amrithmmh
category:
  - uncategorized
date: "2022-09-27T03:30:00+00:00"
guid: https://armphibian.wordpress.com/?p=311
tag:
  - "8051"
  - embedded-c
  - keil
title: 8051 - Embedded C Code (Keil C51 IDE)
url: /2022/09/27/8051-embedded-c-code-keil-c51-ide/

---
**using sbit , reading a single pin and writing to output**

```
#include <reg51.h>

sbit sw=P0^1;
sbit led =P1^1;

// gpio input / output
void main(){

	sw=1;
	while(1){
		led=sw;
	}
}
```

**Mini project: 8051 interfacing with four 7 segment leds , using a timer to count value every 1s**

circuit: (made in proteus)

![](/wp-content/uploads/2022/09/screenshot-2022-09-21-154952.png?w=1024)

**7 segment leds are comman anode (active low) , decoder selects one 7 segment display at a time , p3.3 ,p3.4 are connected to decoder input , p2.0- p2.7 are used to write leds.**

code:

![](/wp-content/uploads/2022/09/imer.png?w=1024)

```
#include <reg51.h>

//sbit should be declared globally
//encoder input 00,01,10,11 --> led 0,1,2,3 --4 led 7 segment selector
sbit d0 = P3 ^ 3;
sbit d1 = P3 ^ 4;

//Port 2 is 7 segment led data input
unsigned char numbers[10] = {
  0xC0,
  0xF9,
  0xA4,
  0xB0,
  0x99,
  0x92,
  0x82,
  0xF8,
  0x80,
  0x90
};

enum segment {
  first,
  second,
  third,
  fourth
};
unsigned char timer_count = 0;
unsigned char digit1 = 0, digit2 = 0, digit3 = 0, digit4 = 0;

void led_write(unsigned char segment, unsigned char number);

void update_time() {
  int i = 0;
  led_write(first, numbers[digit4]);
  for (i = 0; i < 200; i++);
  led_write(second, numbers[digit3]);
  for (i = 0; i < 200; i++);
  led_write(third, numbers[digit2]);
  for (i = 0; i < 200; i++);
  led_write(fourth, numbers[digit1]);
  for (i = 0; i < 200; i++);
}
void timer0_isr() interrupt 1 {
  TH0 = 0x3c;
  TL0 = 0xB0;
  timer_count++;
  if (timer_count == 20) {

    timer_count = 0;
    digit1++;
    if (digit1 == 10) {
      digit1 = 0;
      digit2++;
    }
    if (digit2 == 10) {
      digit2 = 0;
      digit3++;
    }
    if (digit3 == 10) {
      digit3 = 0;
      digit4++;
    }
    if (digit4 == 10) {
      digit1 = 0;
      digit2 = 0;
      digit3 = 0;
      digit4 = 0;
    }
  }
}
void led_write(unsigned char segment, unsigned char number) {

  if (segment == 0) {
    d0 = 0;
    d1 = 0;
  }

  if (segment == 1) {
    d0 = 1;
    d1 = 0;
  }

  if (segment == 2) {
    d0 = 0;
    d1 = 1;
  }
  if (segment == 3) {
    d0 = 1;
    d1 = 1;
  }
  P2 = number;
}

void timer_init() {
  TMOD = 0x01;

  ET0 = 1;
  EA = 1;

  TH0 = 0x3c;
  TL0 = 0xB0;
  TR0 = 1;
}

void main() {

  timer_init();

  while (1) {
    update_time();
  }

  return;

}
```
