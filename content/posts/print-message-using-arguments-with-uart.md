---
author: amrithmmh
category:
  - uncategorized
date: "2023-04-21T07:19:20+00:00"
guid: https://armphibian.wordpress.com/?p=499
title: Print message using arguments with UART
url: /2023/04/21/print-message-using-arguments-with-uart/

---
va\_list, va\_start, va\_end are used for variable argument from header "stdarg.h"

vsprintf is used to combine the va\_list arguments with the message passed to create a single string

code:

```

#include "string.h"
#include "stdlib.h"
#include "stdarg.h"

#define DUART &huart1
#define CUART &huart2

void printmsg(char* msg,...){

#ifdef DEBUG_MSG

	unsigned char buf[100];
	va_list args; 		//list of arguments
	va_start(args,msg); //point to first argument in list

	vsprintf(buf,msg,args); // replace each argument in msg with that of args
	HAL_UART_Transmit_IT(DUART,buf,strlen(buf));
	va_end(args);

#endif

}
```
