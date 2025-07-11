---
author: amrithmmh
category:
  - uncategorized
cover:
  alt: index
  image: /wp-content/uploads/2022/06/index.png
date: "2022-07-04T09:15:51+00:00"
guid: https://armphibian.wordpress.com/?p=209
tag:
  - atmega328p
  - avr
  - embedded-c
title: '[6] Analog (ADC)+USART using AVR (atmega328)'
url: /2022/07/04/6-analog-adcusart-using-avr-atmega328/

---
I am using LDR (light dependent resistor) to get analog values

Below is the explanation from datasheet regarding how to use registers to start one time conversion / free running mode.

> A single conversion is started by disabling the Power Reduction ADC bit, PRADC, in ”Minimizing Power Consumption” on page 51 by writing a logical zero to it and writing a logical one to the ADC Start Conversion bit ADSC. This bit stays high as long as the conversion is in progress and will be cleared by hardware when theconversion is completed. If a different data channel is selected while a conversion is in progress, the ADC will finish the current conversion before performing the channel change.  
Alternatively, a conversion can be triggered automatically by various sources. Auto Triggering is enabled by setting the ADC Auto Trigger Enable bit, ADATE in ADCSRA. The trigger source is selected by setting the ADC Trigger Select bits, ADTS in ADCSRB (See description of the ADTS bits for a list of the trigger sources). When a positive edge occurs on the selected trigger signal, the ADC prescaler is reset and a conversion is started.  
This provides a method of starting conversions at fixed intervals. If the trigger signal still is set when the conversion completes, a new conversion will not be started. If another positive edge occurs on the trigger signal during conversion, the edge will be ignored. Note that an Interrupt Flag will be set even if the specific interrupt is  
disabled or the Global Interrupt Enable bit in SREG is cleared. A conversion can thus be triggered without causing an interrupt. However, the Interrupt Flag must be cleared in order to trigger a new conversion at the next interrupt event.
>
> Using the ADC Interrupt Flag as a trigger source makes the ADC start a new conversion as soon as the ongoing conversion has finished. The ADC then operates in Free Running mode, constantly sampling and updating the ADC Data Register. The first conversion must be started by writing a logical one to the ADSC bit in ADCSRA. In this mode the ADC will perform successive conversions independently of whether the ADC Interrupt Flag, ADIF is cleared or not. If Auto Triggering is enabled, single conversions can be started by writing ADSC in ADCSRA to one. ADSC can also be used to determine if a conversion is in progress. The ADSC bit will be read as one during a conversion,  
independently of how the conversion was started.
>
> -atmega328 datasheet

**Single trigger mode**:

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

void ADCinit(){

ADMUX=0;ADCSRA=0;DIDR0=0;
ADMUX =(1<<REFS0)|(1<<MUX1)|(1<<MUX0);//ADC3,AVCC
ADCSRA =(1<<ADEN)|(1<<ADIE)|(1<<ADPS2)|(1<<ADPS1)|(1<<ADPS0); //enable ADC, enable interrupt
DIDR0=(1<<ADC3D);//disable digital input mode adc3d
ADCSRA |=(1<<ADSC);//start conversion

}

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

int value=0;
int main(void){
USART_init();
ADCinit();
ADCSRA |=(1<<ADSC);
serialWrite("Welcome to AVR\n");

while(1){

	char str[5];
	itoa(value,str,10);
	serialWrite(str);
	serialWrite("\n");
	ADCSRA |=(1<<ADSC);
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

ISR(ADC_vect){
	value=ADC;

}

```
