---
author: amrithmmh
category:
  - uncategorized
date: "2023-02-24T03:16:05+00:00"
draft: "true"
guid: https://armphibian.wordpress.com/?p=494
title: HC-05 (bluetooth classic) / stm32 notes
url: /

---
notes on how HC-05 can be interfaced with stm32 along with some working code

## HC 05 module

HC 05 module is a cheap bluetooth classic module that can be used to build various wireless control projects for students and hobbyists. It has 6 pins and works on UART to send commands from stm32. CAUTION: it is 33v logic level module so beware of how you connect tx and rx pins (use level shifter or voltage divider)
