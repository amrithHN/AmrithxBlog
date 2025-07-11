---
author: amrithmmh
category:
  - uncategorized
date: "2023-02-24T02:56:17+00:00"
guid: https://armphibian.wordpress.com/?p=491
tag:
  - embedded-c
  - esp-idf
  - esp32
  - freertos
title: ESP32 RGB led with PWM / FreeRTOS
url: /2023/02/24/esp32-rgb-led-with-pwm-freertos/

---
Simple code that demonstrate how rgb led is setup and how various colors can be set using the below code

```
/*
 * rgb_led.c
 *
 *  Created on: Jan 28, 2023
 *      Author: amrith
 */

#include"rgb_led.h"

rgbled_handle_t rgb[3];
void led_init(){

	rgb[0].gpio = RGB_LED_1;
	rgb[1].gpio = RGB_LED_2;
	rgb[2].gpio = RGB_LED_3;

	rgb[0].channel = LEDC_CHANNEL_0;
	rgb[1].channel = LEDC_CHANNEL_1;
	rgb[2].channel = LEDC_CHANNEL_2;

	rgb[0].mode = LEDC_HIGH_SPEED_MODE;
	rgb[1].mode = LEDC_HIGH_SPEED_MODE;
	rgb[2].mode = LEDC_HIGH_SPEED_MODE;

	rgb[0].timer = LEDC_TIMER_0;
	rgb[1].timer = LEDC_TIMER_0;
	rgb[2].timer = LEDC_TIMER_0;

	ledc_timer_config_t led_timer_conf = {
			.speed_mode = LEDC_HIGH_SPEED_MODE,
			.duty_resolution = LEDC_TIMER_8_BIT,
			.timer_num = LEDC_TIMER_0,
			.freq_hz = 100
	};

	ledc_timer_config(&led_timer_conf);

	for(int i=0;i<3;i++){
		ledc_channel_config_t led_channel_conf= {
				.gpio_num = rgb[i].gpio,
				.channel = rgb[i].channel,
				.timer_sel = rgb[i].timer,
				.duty = 0,
				.hpoint = 0,
				.intr_type = LEDC_INTR_DISABLE,
				.speed_mode = rgb[i].mode
		};
		ledc_channel_config(&led_channel_conf);
	}

}

void led_set_color(int red, int green , int blue){

ledc_set_duty(rgb[0].mode, rgb[0].channel, red);
ledc_update_duty(rgb[0].mode,rgb[0].channel);

ledc_set_duty(rgb[1].mode, rgb[1].channel, green);
ledc_update_duty(rgb[1].mode,rgb[1].channel);

ledc_set_duty(rgb[2].mode, rgb[2].channel, blue);
ledc_update_duty(rgb[2].mode,rgb[2].channel);

}

```

```
/*
 * rgb_led.h
 *
 *  Created on: Jan 28, 2023
 *      Author: amrith
 */

#ifndef MAIN_RGB_LED_H_
#define MAIN_RGB_LED_H_

#include "driver/ledc.h"
#include "esp_err.h"

#define RGB_LED_1 12
#define RGB_LED_2 14
#define RGB_LED_3 27

typedef struct {
	int gpio;
	int mode;
	int channel;
	int timer;
}rgbled_handle_t;

void led_init();
void led_set_color(int reg, int green , int blue);

#endif /* MAIN_RGB_LED_H_ */

```
