# PIC16-Bit Nano Trainer.

## Features.

- 44 Pins PIC16-Bit Family (dsPIC33/PIC24).
- Powered with micro USB, regulated at 3V3.
- Oscillator internal or external MEMS.
- Headers for breadboard and probes.
- I2C with jumpers.
- UART MCP2221A.
- PCB 21x52mm.

## Schematic.

- [PIC16-Bit Nano Trainer.](./assets/pic16bit-nano.pdf)

## Code Examples.

- [PIC16-Bit Features.](https://github.com/tronixio/trainer-boards/tree/main/features/16bit/)
- [PIC24FJxxGA004 - UART RX/TX Asynchronous.](https://github.com/tronixio/trainer-boards/blob/main/features/16bit/pic24fjxxga00x/eusart.md)

## Jumpers & Capacitor Configuration.

|PIC           |JP1 - VREG +3V3|JP2 - VREG GND|VCAP (C6)|
|--------------|:-------------:|:------------:|:-------:|
|PIC24FJ64GA004|Open           |Close         |Populate |

## Board top.

![PIC16-Bit Nano Top](./pics/pic16bit-nano-top.png)

## Board bottom.

![PIC16-Bit Nano Bottom](./pics/pic16bit-nano-bottom.png)

---