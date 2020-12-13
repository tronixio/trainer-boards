# PIC8-Bit Trainer.

## 0.Contents.

- [1.BARGRAPH - ADC.](#bargraph---adc)
- [2.BARGRAPH - ROTARY ENCODER.](#2bargraph---rotary-encoder)
- [3.TODO - SWITCHES.](#3swithes)

## 1.BARGRAPH - ADC.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1778 - Compile with XC8(v2.31).
// PIC16F1778 - @8MHz Internal Oscillator.
// v0.1 - 05/2020.

// PIC8-Bit Trainer.
// 2x ADC CHANNELS.
// 1x EXPANSION BOARD.
// 1x MODULE SPI.
// 1x MODULE I2C.
// 1X MODULE UART.
// 1x ROTARY ENCODER with SWITCH.
// 4x SWITCHS.

// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MCU.RA0 <- ANALOG.AN1.
// MCU.RC6 -> LED.10.
// MCU.RC7 -> LED.09.
// MCU.RB0 -> LED.08.
// MCU.RB1 -> LED.07.
// MCU.RB2 -> LED.06.
// MCU.RB3 -> LED.05.
// MCU.RB4 -> LED.04.
// MCU.RB5 -> LED.03.
// MCU.RB6 -> LED.02.
// MCU.RB7 -> LED.01.

// Global Variables.
uint8_t u8AdcTimer;

// Interrupt Service Routines.
void __interrupt() ISR(void)
{
    if(INTCONbits.TMR0IF){
        u8AdcTimer++;
        INTCONbits.TMR0IF = 0;
    }
}

// Main.
void main(void)
{
    // MCU Initialization.
    // Internal Oscillator Settings.
    OSCCON = 0x70;
    // Ports Settings.
    // PORT Data Register.
    PORTA = 0b00000000;
    PORTB = 0b00000000;
    PORTC = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000001;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    // WPU Disable.
    OPTION_REGbits.nWPUEN = 0b1;
    // LATCH Outputs.
    LATA = 0b00000000;
    LATB = 0b00000000;
    LATC = 0b00000000;
    // ANSEL Analog.
    ANSELA = 0b00000001;
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

    // ADC Settings.
    ADRESL = 0x00;
    ADRESH = 0x00;
    ADCON0 = 0x00;
    ADCON1 = 0x50;
    ADCON2 = 0x00;
    // ADC Enable.
    ADCON0bits.ADON = 0b1;

    // Timer0 Settings.
    // ~500Hz @8MHz.
    OPTION_REGbits.PS = 0b010;
    OPTION_REGbits.PSA = 0b0;
    OPTION_REGbits.TMR0CS = 0b0;
    // Timer0 Interrupt Enable.
    INTCONbits.TMR0IE = 0b1;
    INTCONbits.TMR0IF = 0b0;

    // Interrupts Enable.
    INTCONbits.GIE = 0b1;

    while(1){
        if(u8AdcTimer>250){
            ADCON0bits.CHS = 0b00000;
            __delay_us(5);
            ADCON0bits.GO = 1;
            while(ADCON0bits.GO){};
            LATB = ADRESH;
            LATC = (LATC & 0x2F) | (ADRESL & 0xC0);
            u8AdcTimer = 0;
        }
    }
}
```

## 2.BARGRAPH - ROTARY ENCODER.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1778 - Compile with XC8(v2.31).
// PIC16F1778 - @8MHz Internal Oscillator.
// v0.1 - 09/2020.

// Rotary encoder code from:
// https://www.mikrocontroller.net/articles/Drehgeber

// PIC8-Bit Trainer.
// 2x ADC CHANNELS.
// 1x EXPANSION BOARD.
// 1x MODULE SPI.
// 1x MODULE I2C.
// 1X MODULE UART.
// 1x ROTARY ENCODER with SWITCH.
// 4x SWITCHS.

// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MCU.RA0 <- ROTARY.A
// MCU.RA0 <- ROTARY.B
// MCU.RA0 <- ROTARY.S5
// MCU.RB0 -> LED.08.
// MCU.RB1 -> LED.07.
// MCU.RB2 -> LED.06.
// MCU.RB3 -> LED.05.
// MCU.RB4 -> LED.04.
// MCU.RB5 -> LED.03.
// MCU.RB6 -> LED.02.
// MCU.RB7 -> LED.01.

// Definitions.
// Rotary Encoder.
#define ROTARY_ENCODER_A         PORTAbits.RA0
#define ROTARY_ENCODER_B         PORTAbits.RA1
#define ROTARY_ENCODER_SWITCH    PORTAbits.RA2

// Function Prototypes.
int8_t rotary_encoderRead(void);

// Global Variables.
int8_t i8encoderDelta;
const int8_t i8encoderFull[16] = {0, 1, -1, 0, -1, 0, 0, 1, 1, 0, 0, -1, 0, -1, 1, 0};

// Interrupt Service Routines.
void __interrupt() ISR(void)
{
    if(INTCONbits.TMR0IF){
        static int8_t i8encoderLast = 0;
        i8encoderLast = (i8encoderLast<<2) & 0x0F;
        if(ROTARY_ENCODER_A) i8encoderLast |= 1;
        if(ROTARY_ENCODER_B) i8encoderLast |= 2;
        i8encoderDelta += i8encoderFull[i8encoderLast];
        INTCONbits.TMR0IF = 0b0;
    }
}

// Main.
void main(void)
{
    // MCU Initialization.
    // Internal Oscillator Settings.
    OSCCON = 0x70;
    // Ports Settings.
    // PORT Data Register.
    PORTA = 0b00000000;
    PORTB = 0b00000000;
    PORTC = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000111;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    // WPU Enable.
    OPTION_REGbits.nWPUEN = 0b0;
    // LATCH Outputs.
    LATA = 0b00000000;
    LATB = 0b00000000;
    LATC = 0b00000000;
    // ANSEL Analog.
    ANSELA = 0b00000000;
    ANSELB = 0b00000000;
    ANSELC = 0b00000000;
    // WPU Weak Pull-up.
    WPUA = 0b00000111;
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

    // Timer0 Settings.
    // ~500Hz @8MHz.
    OPTION_REGbits.PS = 0b010;
    OPTION_REGbits.PSA = 0b0;
    OPTION_REGbits.TMR0CS = 0b0;
    // Timer0 Interrupt Enable.
    INTCONbits.TMR0IE = 0b1;
    INTCONbits.TMR0IF = 0b0;

    // Interrupts Enable.
    INTCONbits.GIE = 0b1;

    uint8_t u8encoderSwitchPressed, u8encoderRead=0, u8encoderLast;
    while(1){
        // ROTARY ENCODER.
        if(!ROTARY_ENCODER_SWITCH){
            __delay_ms(100);
            u8encoderSwitchPressed = 0b1;
        }else if(ROTARY_ENCODER_SWITCH){
            if(u8encoderSwitchPressed){
                u8encoderSwitchPressed = 0b0;
                u8encoderRead = 0;
            }
        }

        u8encoderRead += rotary_encoderRead();
        if(u8encoderLast != u8encoderRead){
            LATB = u8encoderRead;
            u8encoderLast = u8encoderRead;
        }
    }
}

// Functions.
int8_t rotary_encoderRead(void)
{
    int8_t i8encoderRead;

    INTCONbits.TMR0IE = 0b0;
    i8encoderRead = i8encoderDelta;
    i8encoderDelta = i8encoderRead & 3;
    INTCONbits.TMR0IE = 0b1;

    return(i8encoderRead>>2);
}
```

## 3.TODO - SWITCHES.

- Push
- Toggle
- Twice
- Long

---
DISCLAIMER: THIS CODE IS PROVIDED WITHOUT ANY WARRANTY OR GUARANTEES.
USERS MAY USE THIS CODE FOR DEVELOPMENT AND EXAMPLE PURPOSES ONLY.
AUTHORS ARE NOT RESPONSIBLE FOR ANY ERRORS, OMISSIONS, OR DAMAGES THAT COULD
RESULT FROM USING THIS FIRMWARE IN WHOLE OR IN PART.