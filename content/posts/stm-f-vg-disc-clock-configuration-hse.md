---
author: amrithmmh
category:
  - uncategorized
date: "2020-10-09T13:34:22+00:00"
guid: https://armphibian.wordpress.com/?p=47
title: stm32f407vg-disc1 -clock configuration hse
url: /2020/10/09/stm32f407vg-disc1-clock-configuration-hse/

---
code:

```
/**
 ******************************************************************************
 * @file           : main.c
 * @author         : Auto-generated by STM32CubeIDE
 * @brief          : Main program body
 ******************************************************************************
 * @attention
 *
 * <h2><center>© Copyright (c) 2020 STMicroelectronics.
 * All rights reserved.</center></h2>
 *
 * This software component is licensed by ST under BSD 3-Clause license,
 * the "License"; You may not use this file except in compliance with the
 * License. You may obtain a copy of the License at:
 *                        opensource.org/licenses/BSD-3-Clause
 *
 ******************************************************************************
 */

#if !defined(__SOFT_FP__) && defined(__ARM_FP)
  #warning "FPU is not initialized, but the project is compiling for an FPU. Please initialize the FPU before use."
#endif

#define rcc 0x40023800UL
#define rcc_cr_offset 0x00U
#define rcc_cfgr_offset 0x08U
#define rcc_cr (rcc+rcc_cr_offset)
#define rcc_cfgr (rcc+rcc_cfgr_offset)
#include "stdint.h"

int main(void)
{

	uint32_t *rccCR= (uint32_t *)rcc_cr;
	uint32_t *rccCfgr=(uint32_t *)rcc_cfgr;
	*rccCR|=(1<<16);//set 16 bit hseon

	while(!(*rccCR & (1<<17)) );
	*rccCfgr |=(0<<1);//0 bit set
	*rccCfgr &=~(1<<1);//1 bit reset
 //mc01
	*rccCfgr &= ~(0x03<<21);//reset 22 21 bits
	*rccCfgr |=(1<<22); //set 22 bit

	/* Loop forever */
	for(;;);
}

```
