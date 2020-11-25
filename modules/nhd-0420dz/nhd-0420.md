# TODO - NHD-0420DZ-FL-YBW - ST7066U.

## Contents.

- [1.PIC24FJxxGA02 - 8-Bit no Busy Flag.](#)
- [2.PIC24FJxxGA02 - 8-Bit with Busy Flag.](#)
- [3.PIC24FJxxGA02 - 4-Bit no Busy Flag.](#)
- [4.PIC24FJxxGA02 - 4-Bit with Busy Flag.](#)

## 1.PIC24FJxxGA02 - 8-Bit no Busy Flag.

```c
// Configuration Registers.
#pragma config POSCMOD = NONE, I2C1SEL = PRI, IOL1WAY = ON, OSCIOFNC = ON
#pragma config FCKSM = CSDCMD, FNOSC = FRC, SOSCSEL = SOSC, WUTSEL = LEG
#pragma config IESO = OFF, WDTPS = PS32768, FWPSA = PR128, WINDIS = OFF
#pragma config FWDTEN = OFF, ICS = PGx1, GWRP = OFF, GCP = OFF, JTAGEN = OFF

#define FOSC 8000000UL
#define FCY  FOSC/2

#include <xc.h>
#include <libpic30.h>
// PIC24FJxxGA002 - Compile with XC16(v1.60).
// PIC24FJxxGA002 - @8MHz Internal Oscillator.
// v0.1 - 11/2020.

// LCD 4x20 - ST7066U (HD44780 Compatible) - 8-Bit no Busy Flag.
// String & Custom Patterns.

// PIC16-Bit Trainer.
// SDA - Open.
// SCL - Open.
// VREG - GND.
// VCAP - Close.
// VEE - Close.
// BCKL - Close.

// Pinout.
// MODULE.01 -> GND.
// MODULE.02 -> VDD.
// MODULE.03 -> NC.
// MODULE.04 -> MCU.RA0.
// MODULE.05 -> GND.
// MODULE.06 -> MCU.RA2.
// MODULE.07 -> MCU.RB0.
// MODULE.08 -> MCU.RB1.
// MODULE.09 -> MCU.RB2.
// MODULE.10 -> MCU.RB3.
// MODULE.11 -> MCU.RB4.
// MODULE.12 -> MCU.RB5.
// MODULE.13 -> MCU.RB6.
// MODULE.14 -> MCU.RB7.
// MODULE.15 -> NC.
// MODULE.16 -> GND.

// Definitions.
// ST7066U Commands Port & Data Port.
#define ST7066U_DATA                                    LATB
#define ST7066U_RS                                      do{LATAbits.LATA0 = 0b1;} while(0)
#define ST7066U_nRS                                     do{LATAbits.LATA0 = 0b0;} while(0)
#define ST7066U_E                                       do{LATAbits.LATA2 = 0b1;} while(0)
#define ST7066U_nE                                      do{LATAbits.LATA2 = 0b0;} while(0)
// ST7066U Instructions.
#define ST7066U_CLEAR_DISPLAY                           0x01
#define ST7066U_RETURN_HOME                             0x02
#define ST7066U_ENTRY_MODE_DDRAM_DECREMENT_NOSHIFT      0x04
#define ST7066U_ENTRY_MODE_DDRAM_DECREMENT_SHIFT        0x05
#define ST7066U_ENTRY_MODE_DDRAM_INCREMENT_NOSHIFT      0x06
#define ST7066U_ENTRY_MODE_DDRAM_INCREMENT_SHIFT        0x07
#define ST7066U_DISPLAY_OFF                             0x08
#define ST7066U_DISPLAY_ON_CURSOR_OFF                   0x0C
#define ST7066U_DISPLAY_ON_CURSOR_ON_NOBLINK            0x0E
#define ST7066U_DISPLAY_ON_CURSOR_ON_BLINK              0x0F
#define ST7066U_DISPLAY_CURSOR_SHIFT_LEFT               0x10
#define ST7066U_DISPLAY_CURSOR_SHIFT_RIGHT              0x14
#define ST7066U_DISPLAY_SCREEN_SHIFT_LEFT               0x18
#define ST7066U_DISPLAY_SCREEN_SHIFT_RIGHT              0x1C
#define ST7066U_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8     0x20
#define ST7066U_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x11    0x24
#define ST7066U_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8     0x28
#define ST7066U_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8     0x30
#define ST7066U_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x11    0x34
#define ST7066U_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8     0x38
#define ST7066U_SET_CGRAM_ADDRESS                       0x40
#define ST7066U_SET_DDRAM_ADDRESS_FIRST_LINE            0x80
#define ST7066U_SET_DDRAM_ADDRESS_SECOND_LINE           0xC0
#define ST7066U_SET_DDRAM_ADDRESS_THIRD_LINE            0x94
#define ST7066U_SET_DDRAM_ADDRESS_FOURTH_LINE           0xD4

// Functions Prototype.
void st7066u_displayClear(void);
void st7066u_initialize(void);
void st7066u_writeData(char cData);
void st7066u_writeInstruction(char cData);
void st7066u_writeString(const char * cData);
void st7066u_writeStringSetCursor(const char * cData, uint8_t u8Data);

// Strings & Custom patterns.
const char string0[] = "Tronix I/O";
const char string1[] = "www.tronix.io";
const char string2[] = "Custom patterns";
const char custom_patterns[5][8] = {
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x11, 0x11, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f}
};

// Main.
int main(void)
{
    // MCU Initialization.
    // Internal Oscillator Settings.
    CLKDIVbits.RCDIV = 0b000;
    // Ports Initialization.
    // Analog Inputs Settings.
    AD1PCFG = 0b1001111111111111;
    // Port A Settings.
    TRISA = 0b0000000000000000;
    PORTA = 0b0000000000000000;
    LATA = 0b0000000000000000;
    ODCA = 0b0000000000000000;
    // Port B Settings.
    TRISB = 0b0000000000000000;
    PORTB = 0b0000000000000000;
    LATB = 0b0000000000000000;
    ODCB = 0b0000000000000000;

    // ST7066U Initialization.
    st7066u_initialize();

    // Write 5x8 Dots Custom Patterns in ST7066U CGRAM.
    st7066u_writeInstruction(ST7066U_SET_CGRAM_ADDRESS);
    uint8_t line = 0;
    uint8_t pattern = 0;
    for(pattern=0; pattern<5; pattern++){
        for(line=0; line<8; line++){
            st7066u_writeData(custom_patterns[pattern][line]);
        }
    }

    // Display String.
    st7066u_writeStringSetCursor(string0, ST7066U_SET_DDRAM_ADDRESS_FIRST_LINE + 0x05);
    st7066u_writeStringSetCursor(string1, ST7066U_SET_DDRAM_ADDRESS_SECOND_LINE + 0x03);
    st7066u_writeStringSetCursor(string2, ST7066U_SET_DDRAM_ADDRESS_THIRD_LINE + 0x02);

    // Display Custom Patterns from ST7066U CGRAM.
    st7066u_writeInstruction(ST7066U_SET_DDRAM_ADDRESS_FOURTH_LINE + 0x05);
    st7066u_writeData(0x00);
    st7066u_writeData(0x01);
    st7066u_writeData(0x02);
    st7066u_writeData(0x03);
    st7066u_writeData(0x04);
    st7066u_writeString(" - ");

    while(1){
        for(pattern=0; pattern<5; pattern++){
            st7066u_writeInstruction(ST7066U_SET_DDRAM_ADDRESS_FOURTH_LINE + 0x0d);
            st7066u_writeData(pattern);
            __delay_ms(500);
        }
    }
}

// Functions.
void st7066u_displayClear(void)
{
    st7066u_writeInstruction(ST7066U_CLEAR_DISPLAY);
    __delay_ms(2);
}

void st7066u_initialize(void)
{
    __delay_ms(50);
    st7066u_writeInstruction(ST7066U_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8);
    st7066u_writeInstruction(ST7066U_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8);
    st7066u_writeInstruction(ST7066U_DISPLAY_ON_CURSOR_OFF);
    st7066u_writeInstruction(ST7066U_ENTRY_MODE_DDRAM_INCREMENT_NOSHIFT);
    st7066u_writeInstruction(ST7066U_CLEAR_DISPLAY);
    __delay_ms(2);
}

void st7066u_writeData(char cData)
{
    ST7066U_RS;
    ST7066U_E;
    ST7066U_DATA = cData;
    ST7066U_nE;
    __delay_us(40);
}

void st7066u_writeInstruction(char cData)
{
    ST7066U_nRS;
    ST7066U_E;
    ST7066U_DATA = cData;
    ST7066U_nE;
    __delay_us(40);
}

void st7066u_writeString(const char * cData)
{
    while(*cData != '\0')
        st7066u_writeData(*cData++);
}

void st7066u_writeStringSetCursor(const char * cData, uint8_t u8Data)
{
    st7066u_writeInstruction(u8Data);
    while(*cData != '\0')
        st7066u_writeData(*cData++);
}
```

## 2.PIC24FJxxGA02 - 8-Bit with Busy Flag.

```c

```

## 3.PIC24FJxxGA02 - 4-Bit no Busy Flag.

```c

```

## 4.PIC24FJxxGA02 - 4-Bit with Busy Flag.

```c

```

---

DISCLAIMER: THIS CODE IS PROVIDED WITHOUT ANY WARRANTY OR GUARANTEES.
USERS MAY USE THIS CODE FOR DEVELOPMENT AND EXAMPLE PURPOSES ONLY.
AUTHORS ARE NOT RESPONSIBLE FOR ANY ERRORS, OMISSIONS, OR DAMAGES THAT COULD
RESULT FROM USING THIS FIRMWARE IN WHOLE OR IN PART.