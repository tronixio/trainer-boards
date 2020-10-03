# PIC18F2x/4x/5xQ43 - Internal Oscillator.

## 0.Contents.

- [1.LFINTOSC - Low-Frequency Internal Oscillator.](#)
- [2.HFINTOSC @1MHz - High-Frequency Internal Oscillator.](#)
- [3.HFINTOSC @64MHz - High-Frequency Internal Oscillator.](#)

## 1.LFINTOSC - Low-Frequency Internal Oscillator.

```c
// Configuration Registers.
#pragma config FEXTOSC = OFF, RSTOSC = LFINTOSC, CLKOUTEN = ON
#pragma config PR1WAY = ON, CSWEN = ON, FCMEN = OFF, MCLRE = EXTMCLR
#pragma config PWRTS = PWRT_OFF, MVECEN = ON, IVT1WAY = ON, LPBOREN = OFF
#pragma config BOREN = SBORDIS, BORV = VBOR_1P9, ZCD = OFF, PPS1WAY = ON
#pragma config STVREN = ON, LVP = ON, XINST = OFF, WDTCPS = WDTCPS_31, WDTE = ON
#pragma config WDTCWS = WDTCWS_7, WDTCCS = SC, BBSIZE = BBSIZE_512, BBEN = OFF
#pragma config SAFEN = OFF, DEBUG = OFF, WRTB = OFF, WRTC = OFF, WRTD = OFF
#pragma config WRTSAF = OFF, WRTAPP = OFF, CP = OFF

#include <xc.h>
// PIC18F2x/4x/5xQ43 - Compile with XC8(v2.20).
// PIC18F2x/4x/5xQ43 - @xMHz Internal Oscillator.
// v0.1 - 07/2020.

// Internal Clock Source.
// Clock Out Enable on RA6.

// MCU.RA6 -> SCOPE.PROBE.A.

// PIC8-Bit Nano Trainer.
// SDA - Open.
// SCL - Open.

// PIC8-Bit Mini Trainer.
// URX - Open.
// UTX - Open.
// SDA - Open.
// SCL - Open.
// VCAP - Open.
// BCKL - Open.

// PIC8-Bit Trainer.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Main.
int main(void)
{
    // MCU Initialization.
    // Oscillator Settings.
    // Wait Internal Low-Frequency Oscillator Ready.
    while(!OSCSTATbits.LFOR){};

    while(1){
    }
    return(0);
}
```

- FOSC=31kHz.

<p align="center"><img alt=="HFINTOSC" src="./pics/TEK00000.PNG"></p>

## 2.HFINTOSC @1MHz - High-Frequency Internal Oscillator.

```c
```

<p align="center"><img alt=="HFINTOSC" src="./pics/TEK00000.PNG"></p>

## 3.HFINTOSC @64MHz - High-Frequency Internal Oscillator.

```c
```

<p align="center"><img alt=="HFINTOSC" src="./pics/TEK00000.PNG"></p>

---
DISCLAIMER: THIS CODE IS PROVIDED WITHOUT ANY WARRANTY OR GUARANTEES.
USERS MAY USE THIS CODE FOR DEVELOPMENT AND EXAMPLE PURPOSES ONLY.
AUTHORS ARE NOT RESPONSIBLE FOR ANY ERRORS, OMISSIONS, OR DAMAGES THAT COULD
RESULT FROM USING THIS FIRMWARE IN WHOLE OR IN PART.