---
author: amrithmmh
cover:
  alt: vddvss
  image: /wp-content/uploads/2022/09/vddvss.png
date: "2022-09-29T03:30:00+00:00"
guid: https://armphibian.wordpress.com/?p=268
tag:
  - cad
  - kicad
  - pcb
  - pcb-design
  - stm32
title: '[1] Stm32f4xx Custom PCB design using KiCAD v6'
url: /2022/09/29/1-stm32f4xx-custom-pcb-design-using-kicad-v6/

---
Stm32 is a widely popular MCU based on ARM core and I have always been fascinated to work with these tiny powerhouses! This post is a part of the custom PCB design build series and hopefully, I will keep posting about basic hardware design.

Setup a KiCAD project as usual and we will start with the schematics then will check for parts (Do some initial research on main parts required , passive components can be selected later only the size selection matters at this stage).Add stm32f4xx schematic symbol (Its not difficult to make one if the symbol does not exist). Let us divide the whole schematic into multiple section :

1\. Power

2\. External Crystal Oscillator

3\. USB connector (Native USB support)

4\. Connectors for peripheral pins

We use the below two documentation from Stm32 as a reference

1. **AN4488 Application note** Stm32f4xx Hardware design [Guideline](https://www.st.com/resource/en/application_note/dm00115714-getting-started-with-stm32f4xxxx-mcu-hardware-development-stmicroelectronics.pdf)
1. MCU specific datascheet , in my case its Stm32f4xx [Datasheet](https://www.mouser.in/datasheet/2/389/stm32f405rg-1851084.pdf)

## 1\. Power Section

![](/wp-content/uploads/2022/09/power__.png?w=1024)

Mosfet is used to **prevent reverse voltage** protection while the ferrite bead helps to **filter transient voltages** . The schotky diode along with mosfet helps in **switching** between 5V usb line and 12 volt buck converter input.

**Mp2359DJ** is used as the buck converter IC that converts 12v to 3.3V for powering MCU and other components

**2\.** VDD / VSS capacitors

![](/wp-content/uploads/2022/09/vddvss.png?w=670)![](/wp-content/uploads/2022/09/ps_scheme.png?w=1024)

**Based on the documentation one 10u for overall VDD and 100nf for each VDD pins .VDDA required extra ferrite bead to get clean reference voltage**.

\[ Remaining design portion coming soon™ \]
