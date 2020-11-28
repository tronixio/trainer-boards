# DRAFT - PIC16F1777/8/9 - CCP - PWM.

## 0.Contents.

- [1.PIC16F1777/8/9 - CCP - PWM - TIMER FREE RUNING MODE.](#1cpp1278---pwm)

## 1.CPP1/2/7/8 - PWM.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#define _XTAL_FREQ 8000000
// PIC16F1777/8/9 - Compile with XC8(v2.31).
// PIC16F1777/8/9 - @8MHz Internal Oscillator.

// PWM - Timer Free Running Mode.

// PIC16F1778 - CCP1/2/7 - TIMER2.
// PIC16F1777/9 - CCP1/2/7/8 - TIMER2.

// Main.
void main(void)
{
    // MCU Initialization.
    // Oscillator Settings.
    OSCCON = 0x70;
    // Ports Settings.
    // PORT Data Register.
    PORTA = 0b00000000;
    PORTB = 0b00000000;
    PORTC = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000000;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    // WPU Disable.
    OPTION_REGbits.nWPUEN = 0b1;
    // LATCH Outputs.
    LATA = 0b00000000;
    LATB = 0b00000000;
    LATC = 0b00000000;
    // ANSEL Analog.
    ANSELA = 0b00000000;
    ANSELB = 0b00000000;
    ANSELC = 0b00000000;
    // WPU Weak Pull-up.
    WPUA = 0b00000000;
    WPUB = 0b00000000;
    WPUC = 0b00000000;
    // ODCON Open-drain.
    ODCONA = 0b00000000;
    ODCONB = 0b00000000;
    ODCONC = 0b00000000;
    // SRLCON Slew Rate.
    SLRCONA = 0b11111111;
    SLRCONB = 0b11111111;
    SLRCONC = 0b11111111;
    // INLVL Input Level.
    INLVLA  = 0b00000000;
    INLVLB  = 0b00000000;
    INLVLC  = 0b00000000;
    // HIDRVB High Drive.
    HIDRVB  = 0b00000000;
    // PPS Settings.
    PPSLOCK = 0x55;
    PPSLOCK = 0xAA;
    PPSLOCKbits.PPSLOCKED = 0b0;
    // PPS Inputs.
    // PPS Outputs.
    RC6PPSbits.RC6PPS = 0b010101;   // RC6 - CCP1.OUT.
    PPSLOCK = 0x55;
    PPSLOCK = 0xAA;
    PPSLOCKbits.PPSLOCKED = 0b1;

    // Timer2 Settings.
    T2PR = 127;
    //
    T2CONbits.CKPS = 0b000;
    T2CONbits.OUTPS = 0b0000;
    //
    T2HLTbits.PSYNC = 0b0;
    T2HLTbits.CKPOL = 0b0;
    T2HLTbits.CKSYNC = 0b0;
    T2HLTbits.MODE = 0b00000;
    //
    T2CLKCONbits.CS = 0b0001;
    //
    T2RSTbits.RSEL = 0b00000;
    // Timer2 Enable.
    T2CONbits.ON = 0b1;

    // CCP1 Settings.
    //
    CCP1CONbits.FMT = 0b0;
    CCP1CONbits.MODE = 0b1100;
    //
    CCPR1H = 0;
    //
    CCPR1L = 100;
    //
    CCPTMRS1bits.C1TSEL = 0b00;
    // CCP1 Enable.
    CCP1CONbits.CCP1EN = 0b1;
    
    while(1){
    }
}
```

---
DISCLAIMER: THIS CODE IS PROVIDED WITHOUT ANY WARRANTY OR GUARANTEES.
USERS MAY USE THIS CODE FOR DEVELOPMENT AND EXAMPLE PURPOSES ONLY.
AUTHORS ARE NOT RESPONSIBLE FOR ANY ERRORS, OMISSIONS, OR DAMAGES THAT COULD
RESULT FROM USING THIS FIRMWARE IN WHOLE OR IN PART.