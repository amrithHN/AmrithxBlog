---
author: amrithmmh
category:
  - uncategorized
date: "2023-04-26T05:15:04+00:00"
guid: https://armphibian.wordpress.com/?p=477
title: RiscV Workshop day 1
url: /2023/04/26/riscv-workshop-day-1/

---
basic Gcc commands

**riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i filename.c**

**riscv64-unknown-elf-objdump -d a.out \| less**

simulation

**spike pk a.out**

**spike -d pk a.out**

_:until pc 0 100b0_

_100b0_ is the memory location to jump to example main for breakpoint

: reg 0 a2 // read valur of register a2
