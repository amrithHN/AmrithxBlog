---
author: amrithmmh
category:
  - uncategorized
cover:
  alt: IMG_20221003_093302
  image: /wp-content/uploads/2022/10/img_20221003_093302.jpg
date: "2022-10-18T17:49:22+00:00"
guid: https://armphibian.wordpress.com/?p=400
title: '[2] ARM assembly'
url: /2022/10/18/2-arm-assembly/

---
When it comes to programming Arm in thumb2 instructions there are only a handful of opcodes to be considered

![](/wp-content/uploads/2022/10/m3_ins.png?w=1024)

**1.MOV is used to copy data between registers**

ex: **MOV R0,R1**

**2\. Basic load/Store commands**: Used to load /store data between memory and Registers

**MOV R2,=0x20000000**

**LDR R1,\[R2\]**

**STR \[R2\],\[R3\]** //ASSUME R3 HAS A VALID ADDRESS

Variation in LDR and STR include LDRB (load byte), STRB (store byte) , LDRH (Half word 16bit) , STRH (STR half word 16 bit)

**3\. Assembler directives**

- AREA
- IMPORT, EXPORT
- END
- DCD, DCW, DCB
- EQU
- INCLUDE

**AREA** : used to define code space

```
       AREA my_code,CODE,READONLY
__main
       MOV R0,R1

my_func
      MOV R1,R2
```

**IMPORT/EXPORT** : used to export/import a function or data when using multiple assembly files

```
//file1.s
        IMPORT my_func
//inside the program
        BL my_func
```

```
//file2.s
         EXPORT my_func
//function
my_func
         MOV R0,R1
```

**END**\-\- used to mark the end of assembly code

```
 		EXPORT __main
		AREA my_code,CODE,READONLY
__main
		MOV R0,R1
		STR R2,[R0]
HERE	B	HERE
		END
```

defining constant values using **DCD,DCB,DCW , EQU , RN (rename)**

```
		EXPORT __main
		COUNTER RN R0 ; R0 REGISTER IS RENAMED TO COUNTER!
		AREA my_code,CODE,READONLY
__main
		MOV R0,R1
		STR R2,[R0]
HERE		B	HERE

my_data
		dcb 0x30
		dcw 0x1032
		dcd 0x12121212,0x23232323
		END

```

**4\. Arithmetic and logic instructions**

![](/wp-content/uploads/2022/10/m3arith.png?w=1024)![](/wp-content/uploads/2022/10/orm3.png?w=1024)

**5\. Branch instruction**

![](/wp-content/uploads/2022/10/brm3.png?w=1024)
