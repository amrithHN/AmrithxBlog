---
author: amrithmmh
category:
  - uncategorized
date: "2023-04-26T11:35:14+00:00"
guid: https://armphibian.wordpress.com/?p=514
title: Printf redirect to UART2 stm32
url: /2023/04/26/printf-redirect-to-uart2-stm32/

---
There are multiple ways to do this

1. using \_write() override

use they below code to override \_write in printf

```
int _write(int fd, char* ptr, int len) {
    HAL_UART_Transmit(&huart2, (uint8_t *) ptr, len, HAL_MAX_DELAY);
    return len;
}
```

Note: IF you do not see any output use the below code to clear buffer before calling any printf and after UART init

```
setbuf(stdout, NULL);
```

2\. Using Putchar method:

```
#ifdef __GNUC__
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif

PUTCHAR_PROTOTYPE
{
  HAL_UART_Transmit(&huart2, (uint8_t *)&ch, 1, HAL_MAX_DELAY);
  return ch;
}
```

3\. third method using SWV in stm32

enable SWV before using this to see output

make sure syscalls.c is present in the project

```
int __io_putchar(int ch)
{
 // Write character to ITM ch.0
 ITM_SendChar(ch);
 return(ch);
}

or

int _write(int file, char *ptr, int len)
{
  /* Implement your write code here, this is used by puts and printf for example */
  int i=0;
  for(i=0 ; i<len ; i++)
    ITM_SendChar((*ptr++));
  return len;
}
```
