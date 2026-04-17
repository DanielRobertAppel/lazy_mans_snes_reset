# Lazy Man's SNES Reset

"Lazy Man's SNES Reset" is nothing more, than an attiny microcontroller, sniffing the SNES controller port for a certain key combination. [Default: Start + Select + R].

When this button combination is pressed, the microcontroller will set the *RESET-pin* to "high" for a few milli seconds. Is this pin correctly connected to the SNES's reset button, it will trigger a (hard-)reset of the console.

This repository contains everything you need to build your own & become lazy!

![LMSR POC](https://raw.githubusercontent.com/Nold360/lazy_mans_snes_reset/master/doc/lmsr_poc.gif)

# Prerequirements
## Hardware
 - ATtiny13(a)
 - 10k ohm resistor
 - Some kind of ISP-Programmer (e.g. Arduino + ArduinoISP)
 - Some wire, soldering iron, SNES (duh..)

## Software
 - Assuming env: Ubuntu 24.04, using an Arduino Uno
 - Install the Arduino IDE
 - Optional: (If you want to test your Arduino's ability to flash the bootloader onto the ATTiny13A before flashing the hex file)
   - In the Arduino IDE:
     - File -> Preferences:
       - In the Additional Boards Manager URL (with comma separation if you already have values in this field) add:
         - `https://mcudude.github.io/MicroCore/package_MCUdude_MicroCore_index.json`
   - File -> Examples -> 11. Arduino as ISP
   - Upload this sketch to make your Arduino Uno into an ISP for chip programming
   
 - If you want to make changes to the code on the ATTiny13A: Clone this git repository:
```
git clone https://github.com/DanielRobertAppel/lazy_mans_snes_reset.git
cd lazy_mans_snes_reset
```
 - You can also just download the precompiled hex-file & flash it using your prefered software: [Download](https://raw.githubusercontent.com/DanielRobertAppel/lazy_mans_snes_reset/master/lazy_mans_snes_reset.hex)


# Programming the ATTiny13A
Arduino Uno ----> ATTiny13A
```

     13    11
  5V  | 12  |
  |   |  |  |
  v   v  v  v
__8___7__6__5__
|              |
|) AT Tiny13A  |
|______________|
  1  2  3  4
  ^        ^
  |        |
  10      GND
     
```

In your Arduino IDE, Go to:
Tools ->  Ports  
And take note of your arduino's /dev/xxx id.
Launch your terminal and use the following code to flash your ATTiny13A
```
cd /path/where/the/hex/file/is/
avrdude -F -p t13 -P/dev/ttyACM0 -b 19200 -c stk500v1 -U lfuse:w:0x7a:m -U flash:w:lazy_mans_snes_reset.hex:i
avrdude -F -p t13 -P/dev/ttyACM0 -b 19200 -c stk500v1 -U lfuse:w:0x7a:m -U hfuse:w:0xff:m
```



# Advanced (Making your own changes to the attiny13a code)
## Reqired Software
```
sudo apt get install avr-libc gcc-avr avrdude git make gcc
```
## Notable Commands
```
make program
make fuses
```


# Installing the attiny into SNES
**NOTICE:** This guide has been created to the best of my knowledge and certain. But without any guarantee of correctness! **Make sure that you know what you are doing**, as I'm **not** responsible for any damage or harm you might do to your SNES or yourself!

## Attiny Pinout
![attiny pinout](https://raw.githubusercontent.com/Nold360/lazy_mans_snes_reset/master/doc/SNES_Reset_attiny.jpg)

## SNES Junior
![SNES Junior](https://raw.githubusercontent.com/Nold360/lazy_mans_snes_reset/master/doc/LMSR_in_SNES_Junior.jpg)

# Additinal Ressources
 - [Super Nintendo – Pinouts & Protocol FAQ](https://gamefaqs.gamespot.com/snes/916396-super-nintendo/faqs/5395)
 - [Frontplate Pinout](https://i.imgur.com/3deHaFa.png)
