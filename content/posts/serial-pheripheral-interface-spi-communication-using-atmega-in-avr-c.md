---
author: amrithmmh
category:
  - uncategorized
date: "2022-07-08T08:59:21+00:00"
guid: https://armphibian.wordpress.com/?p=228
tag:
  - atmega328p
  - avr
  - embedded-c
title: '[7] Serial Pheripheral Interface(SPI) communication using atmega328 in AVR C'
url: /2022/07/08/7-serial-pheripheral-interfacespi-communication-using-atmega328-in-avr-c/

---
Refer [this](https://maxembedded.com/2013/11/serial-peripheral-interface-spi-basics/) blog for more information \[credit to author\]

**Important registers for SPI**

1.SPCR – SPI Control Register

2.SPSR – SPI Status Register

3.SPDR – SPI Data Register

**atmega328 to atmega328p communication using SPI**:

using PB5,4,3,2 for SPI (arduino uno digital pins 13,12,11,10) SCK,MISO,MOSI,SS

![](/wp-content/uploads/2022/07/atmega328-pinout.png?w=541)

**SPI master using atmega328**

```
#include <avr/io.h>

#define F_CPU 16000000 UL#include <util/delay.h>

void SPI_init() {
  //set gpio as input or output for each SPI pins PB5 SCK output, 4 MISO input, 3 MOSI output ,2 SS output
  DDRB |= (1 << PB5) | (1 << PB3) | (1 << PB2); //output
  DDRB &= ~(1 << PB4);
  PORTB |= (1 << PB2); //write high to slave, low to send data
  SPCR = (1 << SPR0) | (1 << MSTR) | (1 << SPE); // |(1<<SPIE);//Master mode f/16 prescale
}

void Master_send(unsigned char data) {
  //select slave
  PORTB &= ~(1 << PB2); //write zero to ss
  volatile uint8_t spsr = SPSR;
  SPDR = data;
  while (!(SPSR & (1 << SPIF))); //wait
  PORTB |= (1 << PB2);
}

int main(void) {
  SPI_init();

  while (1) {
    //do something

    Master_send('a');
    _delay_ms(2000);

  }

}
```

**Code for Slave arduino:**

```
#include <avr/io.h>
#define F_CPU 16000000 UL
#include <util/delay.h>
#include <avr/interrupt.h>
#define BAUD 9600
#define BRC 103	//9600baud

void USART_init()
{
	UBRR0H = (BRC >> 8);
	UBRR0L = BRC;

	UCSR0B |= (1 << TXEN0) | (1 << TXCIE0);	//1 bit 8 bit data
	//TX setup
	UCSR0C |= (1 << UCSZ01) | (1 << UCSZ00);

}

void send_char(unsigned char data)
{

	while (!(UCSR0A &(1 << UDRE0)));
	UDR0 = data;

}

void SPI_init()
{
	//set gpio as input or output for each SPI pins PB5 SCK input, 4 MISO output, 3 MOSI input ,2 SS input
	DDRB &= ~(1 << PB5);
	DDRB &= ~(1 << PB3);
	DDRB &= ~(1 << PB2);
	DDRB |= (1 << PB4);
	SPCR |= (1 << SPE) | (1 << SPR0);

}
int tmp;
int main(void)
{
	USART_init();
	sei();

	serialWrite("Welcome to AVR\n");
	SPI_init();

	int tmp;
	tmp = SPSR;

	serialWrite("spi initialsed!\n");
	while (1)
	{

		while (!(SPSR &(1 << SPIF)));
		send_char(SPDR);
		int tmp;
		tmp = SPSR;
		_delay_ms(1000);
	}
}

```
