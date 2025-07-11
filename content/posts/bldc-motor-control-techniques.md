---
author: amrithmmh
category:
  - uncategorized
date: "2023-04-26T08:44:19+00:00"
draft: "true"
guid: https://armphibian.wordpress.com/?p=505
title: BLDC motor control techniques
url: /

---
A BLDC motor unlike DC motor require special commutation and excitation techniques to drive it thus making it slighly more complex to control yet it is a superior alternative to brushed DC motor because of few reasons like

1. No brushed means less / no maintenance or breakdown and better life
1. Provides much better torque compared to Brushed DC motor

![](https://www.allaboutcircuits.com/uploads/articles/Brushed_DC_Motor.jpg)

A BLDC motor used in EVs and Quadcopters uses a 3 phase inverter to drive the three phases of the motor. There are two types of arrangements to excite the coils

1. Sensorless
1. Hall sensor based approach

![](https://www.allaboutcircuits.com/uploads/articles/BLDC_DC_Motor_with_Hall_Effect_Sensors-1.jpg)Sensored control of BLDC![](https://www.allaboutcircuits.com/uploads/articles/BLDC_DC_Motor_Sensorless.jpg)Sensorless control of BLDC using back emf

Once a method is chosen a driver is required to excite the coils in order to drive the motor coils, example DRV8323 is a SPI based driver that can be controlled using 1x ,3x, 6x PWM (where 1x PWM is the sensored approach )

Resources/ references

- https://www.renesas.com/us/en/application/key-technology/motor-control-robotics/bldc-motor-control-algorithms
- https://www.allaboutcircuits.com/technical-articles/sensorless-brushless-dc-bldc-motor-control/
- https://www.integrasources.com/blog/bldc-motor-controller-design-principles/
