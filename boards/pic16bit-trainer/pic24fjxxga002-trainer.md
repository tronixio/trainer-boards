# DRAFT - PIC16-Bit Trainer.

## 0.Contents.

- [1.DRAFT - BARGRAPH - ADC.](#bargraph---adc)
- [2.DRAFT - BARGRAPH - ROTARY ENCODER.](#2bargraph---rotary-encoder)
- [3.TODO - SWITCHES.](#3swithes)

## 1.DRAFT - BARGRAPH - ADC.

```c
// Configuration Registers.
#pragma config POSCMOD = NONE, I2C1SEL = PRI, IOL1WAY = ON, OSCIOFNC = ON
#pragma config FCKSM = CSDCMD, FNOSC = FRC, SOSCSEL = SOSC, WUTSEL = LEG
#pragma config IESO = OFF, WDTPS = PS32768, FWPSA = PR128, WINDIS = OFF
#pragma config FWDTEN = OFF, ICS = PGx1, GWRP = OFF, GCP = OFF, JTAGEN = OFF

#define FOSC (8000000UL)
#define FCY (FOSC/2)
#define _ISR_FAST __attribute__ ((interrupt, shadow))
#define _ISR_PSV __attribute__ ((interrupt, auto_psv))
#define _ISR_NOPSV __attribute__ ((interrupt, no_auto_psv))

#include <xc.h>
#include <libpic30.h>
// PIC24FJxxGA002 - Compile with XC16(v1.50).
// PIC24FJxxGA002 - @8MHz Internal Oscillator.
// v0.1 - 09/2020.

// PIC16-Bit Trainer.
// 2x ADC CHANNELS.
// 1x EXPANSION BOARD.
// 1x MODULE SPI.
// 1x MODULE I2C.
// 1X MODULE UART.
// 1x ROTARY ENCODER with SWITCH.
// 4x SWITCHS.

// PIC16-Bit Trainer.
// SDA - Open.
// SCL - Open.
// VREG - GND.
// VCAP - Close.
// VEE - Open.
// BCKL - Open.

// Pinout.
// MCU.RA0 <- ANALOG.AN1.
// MCU.RB0 -> LED.10.
// MCU.RB1 -> LED.09.
// MCU.RB2 -> LED.08.
// MCU.RB3 -> LED.07.
// MCU.RB4 -> LED.06.
// MCU.RB5 -> LED.05.
// MCU.RB6 -> LED.04.
// MCU.RB7 -> LED.03.
// MCU.RB8 -> LED.02.
// MCU.RB9 -> LED.01.

// Global Variables.
uint16_t u16AdcTimer;

// Interrupts Service Routines
void _ISR_NOPSV _T1Interrupt(void)
{
    if(IFS0bits.T1IF){
        u16AdcTimer++;
        IFS0bits.T1IF = 0b0;
    }
}

// Main.
void main(void)
{
    // MCU Initialization.
    // Internal Oscillator Settings.
    CLKDIVbits.RCDIV = 0b000;
    // Ports Initialization.
    // Analog Inputs Settings.
    AD1PCFG = 0b1001111000111110;
    // Port A Settings.
    TRISA = 0b0000000000000001;
    PORTA = 0b0000000000000000;
    LATA = 0b0000000000000000;
    ODCA = 0b0000000000000000;
    // Port B Settings.
    TRISB = 0b0000000000000000;
    PORTB = 0b0000000000000000;
    LATB = 0b0000000000000000;
    ODCB = 0b0000000000000000;

    // Interrupts Settings.
    INTCON1 = 0x8000;
    INTCON2 = 0x0000;

    // ADC Settings.
    AD1CON1 = 0x0000;
    AD1CON1bits.ADSIDL = 0b1;
    AD1CON1bits.FORM = 0b00;
    AD1CON1bits.SSRC = 0b111;
    AD1CON1bits.ASAM = 0b1;
    AD1CON1bits.SAMP = 0b0;
    AD1CON2 = 0x0000;
    AD1CON2bits.VCFG = 0b000;
    AD1CON2bits.CSCNA = 0b1;
    AD1CON2bits.BUFS = 0b0;
    AD1CON2bits.SMPI = 0b0001;
    AD1CON2bits.BUFM = 0b0;
    AD1CON2bits.ALTS = 0b0;
    AD1CON3 = 0x0000;
    AD1CON3bits.ADRC = 0b0;
    AD1CON3bits.SAMC = 0b11111;
    AD1CON3bits.ADCS = 0b00111111;
    AD1CHS = 0x0000;
    AD1CHSbits.CH0NB = 0b0;
    AD1CHSbits.CH0SB = 0b00000;
    AD1CHSbits.CH0NA = 0b0;
    AD1CHSbits.CH0SA = 0b00000;
    AD1CSSL = 0b0000000000000001;
    // ADC Enable.
    AD1CON1bits.ADON = 1;

    // Timer1 Settings.
    // ~500Hz @8MHz.
    PR1 = 3980;
    T1CON = 0x0000;
    // Timer1 Enable.
    T1CONbits.TON = 0b1;
    // Timer1 Interrupts Enable.
    IFS0bits.T1IF = 0b0;
    IEC0bits.T1IE = 0b1;
    IPC0bits.IC1IP = 0b000;

    while(1){
        // ADC Read every ~1s.
        if(u16AdcTimer>1000){
            while(!AD1CON1bits.DONE){};
            LATB = ADC1BUF0;
            u16AdcTimer = 0;
        }
    }
}
```

## 2.DRAFT - BARGRAPH - ROTARY ENCODER.

```c
#pragma config POSCMOD = NONE, I2C1SEL = PRI, IOL1WAY = ON, OSCIOFNC = ON
#pragma config FCKSM = CSDCMD, FNOSC = FRC, SOSCSEL = SOSC, WUTSEL = LEG
#pragma config IESO = OFF, WDTPS = PS32768, FWPSA = PR128, WINDIS = OFF
#pragma config FWDTEN = OFF, ICS = PGx1, GWRP = OFF, GCP = OFF, JTAGEN = OFF

#define FOSC (8000000UL)
#define FCY (FOSC/2)
#define _ISR_FAST __attribute__ ((interrupt, shadow))
#define _ISR_PSV __attribute__ ((interrupt, auto_psv))
#define _ISR_NOPSV __attribute__ ((interrupt, no_auto_psv))

#include <xc.h>
#include <libpic30.h>
// PIC24FJxxGA002 - Compile with XC16(v1.50).
// PIC24FJxxGA002 - @8MHz Internal Oscillator.
// v0.1 - 09/2020.

// PIC16-Bit Trainer.
// 2x ADC CHANNELS.
// 1x EXPANSION BOARD.
// 1x MODULE SPI.
// 1x MODULE I2C.
// 1X MODULE UART.
// 1x ROTARY ENCODER with SWITCH.
// 4x SWITCHS.

// PIC16-Bit Trainer.
// SDA - Open.
// SCL - Open.
// VREG - GND.
// VCAP - Close.
// BCKL - Open.

// Pinout.
// MCU.RA0 <- ROTARY.A - ADD 10K EXTERNAL RESISTOR.
// MCU.RA1 <- ROTARY.B - ADD 10K EXTERNAL RESISTOR.
// MCU.RA2 <- ROTARY.S5 - ADD 10K EXTERNAL RESISTOR.
// MCU.RB0 -> LED.10.
// MCU.RB1 -> LED.09.
// MCU.RB2 -> LED.08.
// MCU.RB3 -> LED.07.
// MCU.RB4 -> LED.06.
// MCU.RB5 -> LED.05.
// MCU.RB6 -> LED.04.
// MCU.RB7 -> LED.03.

// Definitions.
// Rotary Encoder.
#define ROTARY_ENCODER_A        PORTAbits.RA0
#define ROTARY_ENCODER_B        PORTAbits.RA1
#define ROTARY_ENCODER_SWITCH   PORTAbits.RA2

// Function Prototypes.
int8_t rotary_u8encoderRead(void);

// Global Variables.
int8_t i8encoderDelta;
const int8_t i8encoderFull[16] = {0, 1, -1, 0, -1, 0, 0, 1, 1, 0, 0, -1, 0, -1, 1, 0};

// Interrupts Service Routines
void _ISR_NOPSV _T1Interrupt(void)
{
    if(IFS0bits.T1IF){
        static int8_t u8encoderLast = 0;
        u8encoderLast = (u8encoderLast<<2) & 0x0F;
        if(ROTARY_ENCODER_A) u8encoderLast |= 1;
        if(ROTARY_ENCODER_B) u8encoderLast |= 2;
        i8encoderDelta += i8encoderFull[u8encoderLast];
        IFS0bits.T1IF = 0b0;
    }
}

// Main.
void main(void)
{
    // MCU Initialization.
    // Internal Oscillator Settings.
    CLKDIVbits.RCDIV = 0b000;
    // Ports Initialization.
    // Analog Inputs Settings.
    AD1PCFG = 0b1001111000111111;
    // Port A Settings.
    TRISA = 0b0000000000000111;
    PORTA = 0b0000000000000000;
    LATA = 0b0000000000000000;
    ODCA = 0b0000000000000000;
    // Port B Settings.
    TRISB = 0b0000000000000000;
    PORTB = 0b0000000000000000;
    LATB = 0b0000000000000000;
    ODCB = 0b0000000000000000;

    // Interrupts Settings.
    INTCON1 = 0x8000;
    INTCON2 = 0x0000;

    // Timer1 Settings.
    // ~500Hz @8MHz.
    PR1 = 3980;
    T1CON = 0x0000;
    // Timer1 Enable.
    T1CONbits.TON = 0b1;
    // Timer1 Interrupts Enable.
    IFS0bits.T1IF = 0b0;
    IEC0bits.T1IE = 0b1;
    IPC0bits.IC1IP = 0b000;

    uint8_t u8encoderRead=0, u8encoderLast, u8encoderSwitchPressed;
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

        u8encoderRead += rotary_u8encoderRead();
        if(u8encoderLast != u8encoderRead){
            LATB = u8encoderRead;
            u8encoderLast = u8encoderRead;
        }
    }
}

// Functions.
int8_t rotary_u8encoderRead(void)
{
    int8_t u8encoderRead;

    IEC0bits.T1IE = 0b0;
    u8encoderRead = i8encoderDelta;
    i8encoderDelta = u8encoderRead & 3;
    IEC0bits.T1IE = 0b1;

    return(u8encoderRead>>2);
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