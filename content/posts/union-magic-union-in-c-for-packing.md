---
author: amrithmmh
category:
  - uncategorized
date: "2025-02-19T17:39:08+00:00"
guid: https://armphibian.wordpress.com/?p=526
title: 'Union magic : union in C for packing'
url: /2025/02/19/union-magic-union-in-c-for-packing/

---
```

#include <stdio.h>
#include <stdlib.h>
#include "stdint.h"

union {
	uint16_t ADC_16;
	struct{
		uint8_t adc_low;
		uint8_t adc_high;
	}adc_struct;
}ADC_16;

void bitmagic()
{
    ADC_16.adc_struct.adc_low = 0xAD;
    ADC_16.adc_struct.adc_high = 0xDE;
	printf("%x",*((uint16_t*)&ADC_16));
}

int main(void) {

	bitmagic();
	return EXIT_SUCCESS;
}

```

The above code should output **0xDEAD** value when run .

Explanation:

Union allocate the largest members size in memory , and all the members are stacked on top of each other this results in two struct fields pointing to the High byte of 16 bit union , while the other is the low byte

Usage: when reading ADC for example if the registers are held in two different 8bit registers they can be packed as a single 16bit value without any bit shifting or masking.
