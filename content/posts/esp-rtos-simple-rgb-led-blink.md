---
author: amrithmmh
category:
  - uncategorized
date: "2023-01-28T04:29:23+00:00"
guid: https://armphibian.wordpress.com/?p=487
title: ESP32/RTOS -- simple rgb led blink
url: /2023/01/28/esp32-rtos-simple-rgb-led-blink/

---
Below is the code to control 3 gpios in rgb led in esp32

```
/* Blink Example

   This example code is in the Public Domain (or CC0 licensed, at your option.)

   Unless required by applicable law or agreed to in writing, this
   software is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
   CONDITIONS OF ANY KIND, either express or implied.
*/
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"
#include "esp_log.h"
#include "sdkconfig.h"

void configure_led(void)
{

    gpio_reset_pin(12);
    gpio_reset_pin(14);
    gpio_reset_pin(27);
    /* Set the GPIO as a push/pull output */
    gpio_set_direction(12, GPIO_MODE_OUTPUT);
    gpio_set_direction(14, GPIO_MODE_OUTPUT);
    gpio_set_direction(27, GPIO_MODE_OUTPUT);
}

void app_main(void)
{

    /* Configure the peripheral according to the LED type */
    configure_led();

    while (1) {
        /* Toggle the LED state */
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        gpio_set_level(14,0);
        gpio_set_level(27,1);
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        gpio_set_level(27,0);
        gpio_set_level(12,1);
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        gpio_set_level(12,0);
        gpio_set_level(14,1);

    }
}

```
