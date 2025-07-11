---
author: amrithmmh
category:
  - uncategorized
date: "2025-02-19T17:17:30+00:00"
guid: https://armphibian.wordpress.com/?p=522
title: 'Bit magic : Bitfields in C struct'
url: /2025/02/19/bit-magic-bitfields-in-c-struct/

---
```

#include <stdio.h>
#include <stdlib.h>
#include "stdint.h"

typedef struct{
uint8_t bit0:1;
uint8_t bit1:1;
uint8_t bit2:1;
uint8_t bit3:1;
uint8_t bit4:1;
uint8_t bit5:1;
uint8_t bit6:1;
uint8_t bit7:1;
uint8_t bit8:1;
}mybits;

void bitmagic(uint8_t var)
{
	mybits *spi1;
	spi1=(mybits *)&var;
	printf("%b",*((uint8_t *)spi1));
}

int main(void) {

	bitmagic(0xA0);
	return EXIT_SUCCESS;
}

```

Bit fields are very useful to manipulate individual bits and packed with struct

The above code demonstrates a simple example of such usage

Have a great day!
