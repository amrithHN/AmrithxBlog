---
author: amrithmmh
cover:
  alt: index
  image: /wp-content/uploads/2022/06/index.png
date: "2022-07-04T08:02:38+00:00"
guid: https://armphibian.wordpress.com/?p=217
tag:
  - atmega328p
  - avr
  - embedded-c
title: '[5] AVR C USART transmit with ISR using atmega328'
url: /2022/07/04/5-avr-c-usart-transmit-with-isr-using-atmega328/

---
I am using a very simple buffer to keep characters so they are send when USART UDR0 is ready to transmit using an ISR

```
#include <avr/io.h>
#define F_CPU 16000000UL
#include <avr/interrupt.h>

#define BAUD 9600
#define BRC 103 //9600baud

#define tx_buffer_size 128

char serialBuffer[tx_buffer_size];
uint8_t serialReadPosition=0;
uint8_t serialWritePosition=0;

void serialWrite(char c[]);
void serialAppend(char c);

#include <util/delay.h>
#include <avr/interrupt.h>

void serialAppend(char c){

	serialBuffer[serialWritePosition]=c;
	serialWritePosition++;

	if(serialWritePosition>=tx_buffer_size){
		serialWritePosition=0;
	}
}

void serialWrite(char c[]){

	for(uint8_t i=0;i<strlen(c);i++){
		serialAppend(c[i]);
	}

	if (UCSR0A & (1<<UDRE0))
	{
		UDR0=0;
	}

}

void USART_init(){

UBRR0H = (BRC>>8);
UBRR0L = BRC;

UCSR0B|=(1<<TXEN0)|(1<<TXCIE0); //1 bit 8 bit data
//TX setup
UCSR0C |=(1<<UCSZ01)|(1<<UCSZ00);

sei();

}

void send_char(unsigned char data){

	while (!(UCSR0A&(1<<UDRE0)));
	UDR0 =data;

}

int main(void){
USART_init();
while(1){

	serialWrite("Welcome to AVR\n");
	_delay_ms(1000);

}

	return 0;
}

ISR(USART_TX_vect){

	if(serialReadPosition!=serialWritePosition)
	{
		send_char(serialBuffer[serialReadPosition]);
		serialReadPosition++;
	}

	if(serialReadPosition>=tx_buffer_size)
	{
		serialReadPosition=0;
	}
}

```
