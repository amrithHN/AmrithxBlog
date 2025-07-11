---
author: amrithmmh
category:
  - uncategorized
date: "2022-10-10T03:46:20+00:00"
draft: "true"
guid: https://armphibian.wordpress.com/?p=240
title: '[8] I2C (TWI) Two wire interface using atmega328p'
url: /

---
The 2-wire Serial Interface (TWI) is ideally suited for typical microcontroller applications. The TWI protocol  
allows the systems designer to interconnect up to 128 different devices using only two bi-directional bus lines,  
one for clock (SCL) and one for data (SDA). The only external hardware needed to implement the bus is a single  
pull-up resistor for each of the TWI bus lines. All devices connected to the bus have individual addresses, and  
mechanisms for resolving bus contention are inherent in the TWI protocol.

Found [this](https://www.engineersgarage.com/how-to-use-i2c-twi-two-wire-interface-in-avr-atmega32-part-36-46/) interesting blog post explaining TWI in AVR MCUs.

_I am trying to interface an atmega328 with initially atmega328p (uno) for testing_

Modes in TWI:

TWI works in four modes:

1.        MASTER as a transmitter.

2.        MASTER as a receiver.

3.        SLAVE as a receiver.

4.        SLAVE as a transmitter.

(Datasheet explains what registers need to be changed for each operation.)

Master initiaisation:

```

```
