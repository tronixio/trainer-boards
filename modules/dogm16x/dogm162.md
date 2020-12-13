# EA-DOGM162.

## Contents.

- [1.PIC16F1773/6/8 Module EA-DOGM162 - 4-Bit - 3V3.](#1pic16f177368-module-ea-dogm162---4-bit---3v3)
- [2.PIC16F1773/6/8 Module EA-DOGM162 - 8-Bit - 3V3.](#2pic16f177368-module-ea-dogm162---8-bit---3v3)
- [3.PIC16F1773/6/8 Module EA-DOGM162 - SPI - 3V3.](#3pic16f177368-module-ea-dogm162---spi---3v3)
- [4.PIC16F1773/6/8 Module EA-DOGM162 - 4-Bit - 5V.](#4pic16f177368-module-ea-dogm162---4-bit---5v)
- [5.PIC16F1773/6/8 Module EA-DOGM162 - 8-Bit - 5V.](#5pic16f177368-module-ea-dogm162---8-bit---5v)
- [6.PIC16F1773/6/8 Module EA-DOGM162 - SPI - 5V.](#6pic16f177368-module-ea-dogm162---spi---5v)

## 1.PIC16F1773/6/8 Module EA-DOGM162 - 4-Bit - 3V3.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1773/6/8 - Compile with XC8(v2.31).
// PIC16F1773/6/8 - @8MHz Internal Oscillator.

// Module - LCD Character - EA-DOGM162 - ST7036.
// 2 Lines, 8 Characters, 3V3, 4 Bits.
// Strings & Patterns - v0.1 - 05/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MODULE.01 -> GND.
// MODULE.02 -> VDD.
// MODULE.03 -> GND.
// MODULE.04 -> MCU.RA1.
// MODULE.05 -> GND.
// MODULE.06 -> MCU.RA0.
// MODULE.07 -> VDD.
// MODULE.08 -> VDD.
// MODULE.09 -> VDD.
// MODULE.10 -> VDD.
// MODULE.11 -> MCU.RB0.
// MODULE.12 -> MCU.RB1.
// MODULE.13 -> MCU.RB2.
// MODULE.14 -> MCU.RB3..
// MODULE.15 -> MCU.RA2.
// MODULE.16 -> GND.

// Module EA-DOGM081/16X.
// Jumpers.
// PSB - //.
// CAP1 - Close.
// VOUT - Open.

// Definitions.
// ST7036 Commands Port & Data Port.
#define ST7036_DATA                                        LATB
#define ST7036_E                                           do{LATAbits.LATA0 = 0b1;} while(0)
#define ST7036_nE                                          do{LATAbits.LATA0 = 0b0;} while(0)
#define ST7036_RS                                          do{LATAbits.LATA1 = 0b1;} while(0)
#define ST7036_nRS                                         do{LATAbits.LATA1 = 0b0;} while(0)
// ST7036 Instruction Table IS2=0, IS1=0.
#define ST7036_CLEAR_DISPLAY                               0x01
#define ST7036_RETURN_HOME                                 0x02
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_NOSHIFT      0x04
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_SHIFT_RIGHT  0x05
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT      0x06
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_SHIFT_LEFT   0x07
#define ST7036_DISPLAY_OFF                                 0x08
#define ST7036_DISPLAY_ON_CURSOR_OFF                       0x0C
#define ST7036_DISPLAY_ON_CURSOR_ON_NOBLINK                0x0E
#define ST7036_DISPLAY_ON_CURSOR_ON_BLINK                  0x0F
#define ST7036_DISPLAY_CURSOR_SHIFT_LEFT                   0x10
#define ST7036_DISPLAY_CURSOR_SHIFT_RIGHT                  0x14
#define ST7036_DISPLAY_DISPLAY_SHIFT_LEFT                  0x18
#define ST7036_DISPLAY_DISPLAY_SHIFT_RIGH                  0x1C
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8         0x20
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1     0x21
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS2     0x22
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8         0x28
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_DHFONT5x8       0x24
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_DHFONT5x8       0x2C
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8         0x30
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1     0x31
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS2     0x32
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_DHFONT5x8       0x34
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8         0x38
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_DHFONT5x8       0x3C
#define ST7036_SET_ICON_RAM_ADDRESS                        0X40
#define ST7036_SET_CGRAM_ADDRESS                           0x40
#define ST7036_DDRAM_ADDRESS_FIRST_LINE                    0x80
#define ST7036_DDRAM_ADDRESS_SECOND_LINE                   0xC0
// ST7036 Instruction Table IS2=0, IS1=1.
#define ST7036_BIAS_SET_1_5                                0x14
#define ST7036_BIAS_SET_1_5_3_LINE                         0x15
#define ST7036_BIAS_SET_1_4                                0x1C
#define ST7036_BIAS_SET_1_4_3_LINE                         0x1D
#define ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST        0x50
#define ST7036_POWER_ICON_ON_BOOST_OFF_NO_CONTRAST         0x58
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_0      0x54
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1      0x55
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_2      0x56
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_3      0x57
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_0       0x5C
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_1       0x5D
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_2       0x5E
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_3       0x5F
#define ST7036_FOLLOWER_CONTROL_OFF                        0x60
#define ST7036_FOLLOWER_CONTROL_ON_RAB_0                   0x68
#define ST7036_FOLLOWER_CONTROL_ON_RAB_1                   0x69
#define ST7036_FOLLOWER_CONTROL_ON_RAB_2                   0x6A
#define ST7036_FOLLOWER_CONTROL_ON_RAB_3                   0x6B
#define ST7036_FOLLOWER_CONTROL_ON_RAB_4                   0x6C
#define ST7036_FOLLOWER_CONTROL_ON_RAB_5                   0x6D
#define ST7036_FOLLOWER_CONTROL_ON_RAB_6                   0x6E
#define ST7036_FOLLOWER_CONTROL_ON_RAB_7                   0x6F
#define ST7036_CONTRAST_LSB_0                              0x70
#define ST7036_CONTRAST_LSB_1                              0x71
#define ST7036_CONTRAST_LSB_2                              0x72
#define ST7036_CONTRAST_LSB_3                              0x73
#define ST7036_CONTRAST_LSB_4                              0x74
#define ST7036_CONTRAST_LSB_5                              0x75
#define ST7036_CONTRAST_LSB_6                              0x76
#define ST7036_CONTRAST_LSB_7                              0x77
#define ST7036_CONTRAST_LSB_8                              0x78
#define ST7036_CONTRAST_LSB_9                              0x79
#define ST7036_CONTRAST_LSB_10                             0x7A
#define ST7036_CONTRAST_LSB_11                             0x7B
#define ST7036_CONTRAST_LSB_12                             0x7C
#define ST7036_CONTRAST_LSB_13                             0x7D
#define ST7036_CONTRAST_LSB_14                             0x7E
#define ST7036_CONTRAST_LSB_15                             0x7F
#define ST7036_INSTRUCTION_DELAY_US                        28
#define ST7036_CLEAR_DISPLAY_DELAY_MS                      1
#define ST7036_INITIALIZATION_DELAY_MS                     30
// ST7036 Instruction Table IS2=1, IS1=0.
#define ST7036_DOUBLE_HEIGHT_FONT_COM9_COM24               0x10
#define ST7036_DOUBLE_HEIGHT_FONT_COM1_COM16               0x11
// ASCII Characters.
#define ASCII_SPACE                                        0x20
// EA-DOGM Configuration.
#define DOGM_CONFIGURATION_FIRST_LINE                      ST7036_DDRAM_ADDRESS_FIRST_LINE
#define DOGM_CONFIGURATION_SECOND_LINE                     ST7036_DDRAM_ADDRESS_SECOND_LINE
#define DOGM_CONFIGURATION_CHARACTERS                      16
#define DOGM_BACKLIGHT_OFF                                 do{LATAbits.LATA2 = 0b0;} while(0)
#define DOGM_BACKLIGHT_ON                                  do{LATAbits.LATA2 = 0b1;} while(0)

// Function Prototype.
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Line);
void lcd_initialization(void);
void lcd_writeData(uint8_t u8Data);
void lcd_writeInstruction(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor);

// Strings & Custom Patterns.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8BatteryPattern[5][8] = {
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x11, 0x11, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f}
};

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
    PORTE = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000000;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    TRISE = 0b00000000;
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
    WPUE = 0b00000000;
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

    // ST7036 Initialization.
    lcd_initialization();

    // Write 5x8 Dots Custom Patterns in ST7036 CGRAM.
    uint8_t line, pattern;
    lcd_writeInstruction(ST7036_SET_CGRAM_ADDRESS);
    for(pattern=0; pattern<5; pattern++){
        for(line=0; line<8; line++){
            lcd_writeData(au8BatteryPattern[pattern][line]);
       }
    }

    // Display Strings.
    lcd_writeStringSetCursor(au8Tronix, DOGM_CONFIGURATION_FIRST_LINE);
    lcd_writeStringSetCursor(au8WWW, DOGM_CONFIGURATION_SECOND_LINE);

    DOGM_BACKLIGHT_ON;
    while(1){
        for(pattern=0; pattern<5; pattern++){
            lcd_writeInstruction(DOGM_CONFIGURATION_FIRST_LINE + 0x0f);
            lcd_writeData(pattern);
            __delay_ms(500);
        }
    }
}

// Functions.
void lcd_clearDisplay(void)
{
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_clearLine(uint8_t u8Line)
{
    uint8_t character = DOGM_CONFIGURATION_CHARACTERS;

    lcd_writeInstruction(u8Line);
    while(character--)
        lcd_writeData(ASCII_SPACE);
    lcd_writeInstruction(u8Line);
}

void lcd_initialization(void)
{
    __delay_ms(ST7036_INITIALIZATION_DELAY_MS);
    // Instruction Table IS2=0, IS1=1.
    lcd_writeInstruction(ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1);
    lcd_writeInstruction(ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1);
    lcd_writeInstruction(ST7036_BIAS_SET_1_5);
    lcd_writeInstruction(ST7036_CONTRAST_LSB_7);
    lcd_writeInstruction(ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1);
    lcd_writeInstruction(ST7036_FOLLOWER_CONTROL_ON_RAB_5);
    // Instruction Table IS2=0, IS1=0.
    lcd_writeInstruction(ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8);
    lcd_writeInstruction(ST7036_DISPLAY_ON_CURSOR_OFF);
    lcd_writeInstruction(ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT);
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_writeData(uint8_t u8Data)
{
    ST7036_RS;

    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= ((u8Data>>4) & 0x0f);
    ST7036_nE;
    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= (u8Data & 0x0f);
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeInstruction(uint8_t u8Data)
{
    ST7036_nRS;

    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= ((u8Data>>4) & 0x0f);
    ST7036_nE;
    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= (u8Data & 0x0f);
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeString(const uint8_t * u8Data)
{
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor)
{
    lcd_writeInstruction(u8Cursor);
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}
```

## 2.PIC16F1773/6/8 Module EA-DOGM162 - 8-Bit - 3V3.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1773/6/8 - Compile with XC8(v2.31).
// PIC16F1773/6/8 - @8MHz Internal Oscillator.

// Module - LCD Character - EA-DOGM162 - ST7036.
// 2 Lines, 8 Characters, 3V3, 8 Bits.
// Strings & Patterns - v0.1 - 05/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MODULE.01 -> GND.
// MODULE.02 -> VDD.
// MODULE.03 -> GND.
// MODULE.04 -> MCU.RA1.
// MODULE.05 -> GND.
// MODULE.06 -> MCU.RA0.
// MODULE.07 -> MCU.RB0.
// MODULE.08 -> MCU.RB1.
// MODULE.09 -> MCU.RB2.
// MODULE.10 -> MCU.RB3.
// MODULE.11 -> MCU.RB4.
// MODULE.12 -> MCU.RB5.
// MODULE.13 -> MCU.RB6.
// MODULE.14 -> MCU.RB7.
// MODULE.15 -> MCU.RA2.
// MODULE.16 -> GND.

// Module EA-DOGM081/16X.
// Jumpers.
// PSB - //.
// CAP1 - Close.
// VOUT - Open.

// Definitions.
// ST7036 Commands Port & Data Port.
#define ST7036_DATA                                        LATB
#define ST7036_E                                           do{LATAbits.LATA0 = 0b1;} while(0)
#define ST7036_nE                                          do{LATAbits.LATA0 = 0b0;} while(0)
#define ST7036_RS                                          do{LATAbits.LATA1 = 0b1;} while(0)
#define ST7036_nRS                                         do{LATAbits.LATA1 = 0b0;} while(0)
// ST7036 Instruction Table IS2=0, IS1=0.
#define ST7036_CLEAR_DISPLAY                               0x01
#define ST7036_RETURN_HOME                                 0x02
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_NOSHIFT      0x04
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_SHIFT_RIGHT  0x05
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT      0x06
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_SHIFT_LEFT   0x07
#define ST7036_DISPLAY_OFF                                 0x08
#define ST7036_DISPLAY_ON_CURSOR_OFF                       0x0C
#define ST7036_DISPLAY_ON_CURSOR_ON_NOBLINK                0x0E
#define ST7036_DISPLAY_ON_CURSOR_ON_BLINK                  0x0F
#define ST7036_DISPLAY_CURSOR_SHIFT_LEFT                   0x10
#define ST7036_DISPLAY_CURSOR_SHIFT_RIGHT                  0x14
#define ST7036_DISPLAY_DISPLAY_SHIFT_LEFT                  0x18
#define ST7036_DISPLAY_DISPLAY_SHIFT_RIGH                  0x1C
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8         0x20
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1     0x21
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS2     0x22
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8         0x28
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_DHFONT5x8       0x24
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_DHFONT5x8       0x2C
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8         0x30
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1     0x31
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS2     0x32
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_DHFONT5x8       0x34
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8         0x38
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_DHFONT5x8       0x3C
#define ST7036_SET_ICON_RAM_ADDRESS                        0X40
#define ST7036_SET_CGRAM_ADDRESS                           0x40
#define ST7036_DDRAM_ADDRESS_FIRST_LINE                    0x80
#define ST7036_DDRAM_ADDRESS_SECOND_LINE                   0xC0
// ST7036 Instruction Table IS2=0, IS1=1.
#define ST7036_BIAS_SET_1_5                                0x14
#define ST7036_BIAS_SET_1_5_3_LINE                         0x15
#define ST7036_BIAS_SET_1_4                                0x1C
#define ST7036_BIAS_SET_1_4_3_LINE                         0x1D
#define ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST        0x50
#define ST7036_POWER_ICON_ON_BOOST_OFF_NO_CONTRAST         0x58
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_0      0x54
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1      0x55
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_2      0x56
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_3      0x57
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_0       0x5C
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_1       0x5D
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_2       0x5E
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_3       0x5F
#define ST7036_FOLLOWER_CONTROL_OFF                        0x60
#define ST7036_FOLLOWER_CONTROL_ON_RAB_0                   0x68
#define ST7036_FOLLOWER_CONTROL_ON_RAB_1                   0x69
#define ST7036_FOLLOWER_CONTROL_ON_RAB_2                   0x6A
#define ST7036_FOLLOWER_CONTROL_ON_RAB_3                   0x6B
#define ST7036_FOLLOWER_CONTROL_ON_RAB_4                   0x6C
#define ST7036_FOLLOWER_CONTROL_ON_RAB_5                   0x6D
#define ST7036_FOLLOWER_CONTROL_ON_RAB_6                   0x6E
#define ST7036_FOLLOWER_CONTROL_ON_RAB_7                   0x6F
#define ST7036_CONTRAST_LSB_0                              0x70
#define ST7036_CONTRAST_LSB_1                              0x71
#define ST7036_CONTRAST_LSB_2                              0x72
#define ST7036_CONTRAST_LSB_3                              0x73
#define ST7036_CONTRAST_LSB_4                              0x74
#define ST7036_CONTRAST_LSB_5                              0x75
#define ST7036_CONTRAST_LSB_6                              0x76
#define ST7036_CONTRAST_LSB_7                              0x77
#define ST7036_CONTRAST_LSB_8                              0x78
#define ST7036_CONTRAST_LSB_9                              0x79
#define ST7036_CONTRAST_LSB_10                             0x7A
#define ST7036_CONTRAST_LSB_11                             0x7B
#define ST7036_CONTRAST_LSB_12                             0x7C
#define ST7036_CONTRAST_LSB_13                             0x7D
#define ST7036_CONTRAST_LSB_14                             0x7E
#define ST7036_CONTRAST_LSB_15                             0x7F
#define ST7036_INSTRUCTION_DELAY_US                        28
#define ST7036_CLEAR_DISPLAY_DELAY_MS                      1
#define ST7036_INITIALIZATION_DELAY_MS                     30
// ST7036 Instruction Table IS2=1, IS1=0.
#define ST7036_DOUBLE_HEIGHT_FONT_COM9_COM24               0x10
#define ST7036_DOUBLE_HEIGHT_FONT_COM1_COM16               0x11
// ASCII Characters.
#define ASCII_SPACE                                        0x20
// EA-DOGM Configuration.
#define DOGM_CONFIGURATION_FIRST_LINE                      ST7036_DDRAM_ADDRESS_FIRST_LINE
#define DOGM_CONFIGURATION_SECOND_LINE                     ST7036_DDRAM_ADDRESS_SECOND_LINE
#define DOGM_CONFIGURATION_CHARACTERS                      16
#define DOGM_BACKLIGHT_OFF                                 do{LATAbits.LATA2 = 0b0;} while(0)
#define DOGM_BACKLIGHT_ON                                  do{LATAbits.LATA2 = 0b1;} while(0)

// Function Prototype.
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Line);
void lcd_initialization(void);
void lcd_writeData(uint8_t u8Data);
void lcd_writeInstruction(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor);

// Strings & Custom Patterns.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8BatteryPattern[5][8] = {
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x11, 0x11, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f}
};

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
    PORTE = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000000;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    TRISE = 0b00000000;
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
    WPUE = 0b00000000;
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

    // ST7036 Initialization.
    lcd_initialization();

    // Write 5x8 Dots Custom Patterns in ST7036 CGRAM.
    uint8_t line, pattern;
    lcd_writeInstruction(ST7036_SET_CGRAM_ADDRESS);
    for(pattern=0; pattern<5; pattern++){
        for(line=0; line<8; line++){
            lcd_writeData(au8BatteryPattern[pattern][line]);
       }
    }

    // Display Strings.
    lcd_writeStringSetCursor(au8Tronix, DOGM_CONFIGURATION_FIRST_LINE);
    lcd_writeStringSetCursor(au8WWW, DOGM_CONFIGURATION_SECOND_LINE);

    DOGM_BACKLIGHT_ON;
    while(1){
        for(pattern=0; pattern<5; pattern++){
            lcd_writeInstruction(DOGM_CONFIGURATION_FIRST_LINE + 0x0f);
            lcd_writeData(pattern);
            __delay_ms(500);
        }
    }
}

// Functions.
void lcd_clearDisplay(void)
{
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_clearLine(uint8_t u8Line)
{
    uint8_t character = DOGM_CONFIGURATION_CHARACTERS;

    lcd_writeInstruction(u8Line);
    while(character--)
        lcd_writeData(ASCII_SPACE);
    lcd_writeInstruction(u8Line);
}

void lcd_initialization(void)
{
    __delay_ms(ST7036_INITIALIZATION_DELAY_MS);
    // Instruction Table IS2=0, IS1=1.
    lcd_writeInstruction(ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1);
    lcd_writeInstruction(ST7036_BIAS_SET_1_5);
    lcd_writeInstruction(ST7036_CONTRAST_LSB_7);
    lcd_writeInstruction(ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1);
    lcd_writeInstruction(ST7036_FOLLOWER_CONTROL_ON_RAB_5);
    // Instruction Table IS2=0, IS1=0.
    lcd_writeInstruction(ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8);
    lcd_writeInstruction(ST7036_DISPLAY_ON_CURSOR_OFF);
    lcd_writeInstruction(ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT);
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_writeData(uint8_t u8Data)
{
    ST7036_RS;
    ST7036_E;
    ST7036_DATA = u8Data;
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeInstruction(uint8_t u8Data)
{
    ST7036_nRS;
    ST7036_E;
    ST7036_DATA = u8Data;
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeString(const uint8_t * u8Data)
{
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor)
{
    lcd_writeInstruction(u8Cursor);
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}
```

## 3.PIC16F1773/6/8 Module EA-DOGM162 - SPI - 3V3.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1773/6/8 - Compile with XC8(v2.31).
// PIC16F1773/6/8 - @8MHz Internal Oscillator.

// Module - LCD Character - EA-DOGM162 - ST7036.
// 2 Lines, 8 Characters, 3V3, SPI.
// Strings & Patterns - v0.1 - 05/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MODULE.01 -> GND.
// MODULE.02 -> VDD.
// MODULE.03 -> MCU.RA0.
// MODULE.04 -> MCU.RA1.
// MODULE.05 -> GND.
// MODULE.06 -> VDD.
// MODULE.07 -> VDD.
// MODULE.08 -> VDD.
// MODULE.09 -> VDD.
// MODULE.10 -> VDD.
// MODULE.11 -> VDD.
// MODULE.12 -> VDD.
// MODULE.13 -> MCU.RC3.
// MODULE.14 -> MCU.RC5.
// MODULE.15 -> MCU.RA2.
// MODULE.16 -> GND.

// Module EA-DOGM081/16X.
// Jumpers.
// PSB - SPI.
// CAP1 - Close.
// VOUT - Open.

// Definitions.
// SPI.
#define SPI_CS                                             do{LATAbits.LATA0 = 0b1;} while(0)
#define SPI_nCS                                            do{LATAbits.LATA0 = 0b0;} while(0)
// ST7036 Commands Port.
#define ST7036_RS                                          do{LATAbits.LATA1 = 0b1;} while(0)
#define ST7036_nRS                                         do{LATAbits.LATA1 = 0b0;} while(0)
// ST7036 Instruction Table IS2=0, IS1=0.
#define ST7036_CLEAR_DISPLAY                               0x01
#define ST7036_RETURN_HOME                                 0x02
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_NOSHIFT      0x04
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_SHIFT_RIGHT  0x05
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT      0x06
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_SHIFT_LEFT   0x07
#define ST7036_DISPLAY_OFF                                 0x08
#define ST7036_DISPLAY_ON_CURSOR_OFF                       0x0C
#define ST7036_DISPLAY_ON_CURSOR_ON_NOBLINK                0x0E
#define ST7036_DISPLAY_ON_CURSOR_ON_BLINK                  0x0F
#define ST7036_DISPLAY_CURSOR_SHIFT_LEFT                   0x10
#define ST7036_DISPLAY_CURSOR_SHIFT_RIGHT                  0x14
#define ST7036_DISPLAY_DISPLAY_SHIFT_LEFT                  0x18
#define ST7036_DISPLAY_DISPLAY_SHIFT_RIGH                  0x1C
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8         0x20
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1     0x21
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS2     0x22
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8         0x28
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_DHFONT5x8       0x24
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_DHFONT5x8       0x2C
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8         0x30
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1     0x31
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS2     0x32
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_DHFONT5x8       0x34
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8         0x38
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_DHFONT5x8       0x3C
#define ST7036_SET_ICON_RAM_ADDRESS                        0X40
#define ST7036_SET_CGRAM_ADDRESS                           0x40
#define ST7036_DDRAM_ADDRESS_FIRST_LINE                    0x80
#define ST7036_DDRAM_ADDRESS_SECOND_LINE                   0xC0
// ST7036 Instruction Table IS2=0, IS1=1.
#define ST7036_BIAS_SET_1_5                                0x14
#define ST7036_BIAS_SET_1_5_3_LINE                         0x15
#define ST7036_BIAS_SET_1_4                                0x1C
#define ST7036_BIAS_SET_1_4_3_LINE                         0x1D
#define ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST        0x50
#define ST7036_POWER_ICON_ON_BOOST_OFF_NO_CONTRAST         0x58
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_0      0x54
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1      0x55
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_2      0x56
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_3      0x57
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_0       0x5C
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_1       0x5D
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_2       0x5E
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_3       0x5F
#define ST7036_FOLLOWER_CONTROL_OFF                        0x60
#define ST7036_FOLLOWER_CONTROL_ON_RAB_0                   0x68
#define ST7036_FOLLOWER_CONTROL_ON_RAB_1                   0x69
#define ST7036_FOLLOWER_CONTROL_ON_RAB_2                   0x6A
#define ST7036_FOLLOWER_CONTROL_ON_RAB_3                   0x6B
#define ST7036_FOLLOWER_CONTROL_ON_RAB_4                   0x6C
#define ST7036_FOLLOWER_CONTROL_ON_RAB_5                   0x6D
#define ST7036_FOLLOWER_CONTROL_ON_RAB_6                   0x6E
#define ST7036_FOLLOWER_CONTROL_ON_RAB_7                   0x6F
#define ST7036_CONTRAST_LSB_0                              0x70
#define ST7036_CONTRAST_LSB_1                              0x71
#define ST7036_CONTRAST_LSB_2                              0x72
#define ST7036_CONTRAST_LSB_3                              0x73
#define ST7036_CONTRAST_LSB_4                              0x74
#define ST7036_CONTRAST_LSB_5                              0x75
#define ST7036_CONTRAST_LSB_6                              0x76
#define ST7036_CONTRAST_LSB_7                              0x77
#define ST7036_CONTRAST_LSB_8                              0x78
#define ST7036_CONTRAST_LSB_9                              0x79
#define ST7036_CONTRAST_LSB_10                             0x7A
#define ST7036_CONTRAST_LSB_11                             0x7B
#define ST7036_CONTRAST_LSB_12                             0x7C
#define ST7036_CONTRAST_LSB_13                             0x7D
#define ST7036_CONTRAST_LSB_14                             0x7E
#define ST7036_CONTRAST_LSB_15                             0x7F
#define ST7036_INSTRUCTION_DELAY_US                        28
#define ST7036_CLEAR_DISPLAY_DELAY_MS                      1
#define ST7036_INITIALIZATION_DELAY_MS                     30
// ST7036 Instruction Table IS2=1, IS1=0.
#define ST7036_DOUBLE_HEIGHT_FONT_COM9_COM24               0x10
#define ST7036_DOUBLE_HEIGHT_FONT_COM1_COM16               0x11
// ASCII Characters.
#define ASCII_SPACE                                        0x20
// EA-DOGM Configuration.
#define DOGM_CONFIGURATION_FIRST_LINE                      ST7036_DDRAM_ADDRESS_FIRST_LINE
#define DOGM_CONFIGURATION_SECOND_LINE                     ST7036_DDRAM_ADDRESS_SECOND_LINE
#define DOGM_CONFIGURATION_CHARACTERS                      16
#define DOGM_BACKLIGHT_OFF                                 do{LATAbits.LATA2 = 0b0;} while(0)
#define DOGM_BACKLIGHT_ON                                  do{LATAbits.LATA2 = 0b1;} while(0)

// Function Prototype.
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Line);
void lcd_initialization(void);
void lcd_writeData(uint8_t u8Data);
void lcd_writeInstruction(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor);

// Strings & Custom Patterns.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8BatteryPattern[5][8] = {
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x11, 0x11, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f}
};

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
    PORTE = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000000;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    TRISE = 0b00000000;
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
    WPUE = 0b00000000;
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
    // PPS Outputs.
    RC3PPSbits.RC3PPS = 0x21;    // RC3 - MSSP.SCK.
    RC5PPSbits.RC5PPS = 0x23;    // RC5 - MSSP.SDO.
    PPSLOCK = 0x55;
    PPSLOCK = 0xAA;
    PPSLOCKbits.PPSLOCKED = 0b1;

    // SPI Mater Settings.
    // Mode 0 - Fsck = Fosc/16.
    SSP1BUF = 0;
    SSP1ADD = 0;
    SSP1MSK = 0;
    SSP1STAT = 0x40;
    SSP1CON1 = 0x01;
    SSP1CON2 = 0x00;
    SSP1CON3 = 0x00;
    // SPI Enable.
    SSP1CON1bits.SSPEN = 0b1;

    // ST7036 Initialization.
    lcd_initialization();

    // Write 5x8 Dots Custom Patterns in ST7036 CGRAM.
    uint8_t line, pattern;
    lcd_writeInstruction(ST7036_SET_CGRAM_ADDRESS);
    for(pattern=0; pattern<5; pattern++){
        for(line=0; line<8; line++){
            lcd_writeData(au8BatteryPattern[pattern][line]);
       }
    }

    // Display Strings.
    lcd_writeStringSetCursor(au8Tronix, DOGM_CONFIGURATION_FIRST_LINE);
    lcd_writeStringSetCursor(au8WWW, DOGM_CONFIGURATION_SECOND_LINE);

    DOGM_BACKLIGHT_ON;
    while(1){
        for(pattern=0; pattern<5; pattern++){
            lcd_writeInstruction(DOGM_CONFIGURATION_FIRST_LINE + 0x0f);
            lcd_writeData(pattern);
            __delay_ms(500);
        }
    }
}

// Functions.
void lcd_clearDisplay(void)
{
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_clearLine(uint8_t u8Line)
{
    uint8_t character = DOGM_CONFIGURATION_CHARACTERS;

    lcd_writeInstruction(u8Line);
    while(character--)
        lcd_writeData(ASCII_SPACE);
    lcd_writeInstruction(u8Line);
}

void lcd_initialization(void)
{
    uint8_t u8Dummy;

    SPI_nCS;
    ST7036_nRS;
    __delay_ms(ST7036_INITIALIZATION_DELAY_MS);
    // Instruction Table IS2=0, IS1=1.
    SSPBUF = ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_BIAS_SET_1_5;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_CONTRAST_LSB_7;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_FOLLOWER_CONTROL_ON_RAB_5;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    // Instruction Table IS2=0, IS1=0.
    SSPBUF = ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_DISPLAY_ON_CURSOR_OFF;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_CLEAR_DISPLAY;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SPI_CS;
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_writeData(uint8_t u8Data)
{
    uint8_t u8Dummy;

    SPI_nCS;
    ST7036_RS;
    SSP1BUF = (u8Data);
    while(!SSP1STATbits.BF){};
    u8Dummy = SSP1BUF;
    SPI_CS;
}

void lcd_writeInstruction(uint8_t u8Data)
{
    uint8_t u8Dummy;

    SPI_nCS;
    ST7036_nRS;
    SSP1BUF = (u8Data);
    while(!SSP1STATbits.BF){};
    u8Dummy = SSP1BUF;
    SPI_CS;
}

void lcd_writeString(const uint8_t * u8Data)
{
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor)
{
    lcd_writeInstruction(u8Cursor);
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}
```

## 4.PIC16F1773/6/8 Module EA-DOGM162 - 4-Bit - 5V.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1773/6/8 - Compile with XC8(v2.31).
// PIC16F1773/6/8 - @8MHz Internal Oscillator.

// Module - LCD Character - EA-DOGM162 - ST7036.
// 2 Lines, 8 Characters, 5V, 4 Bits.
// Strings & Patterns - v0.1 - 05/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MODULE.01 -> GND.
// MODULE.02 -> VDD.
// MODULE.03 -> GND.
// MODULE.04 -> MCU.RA1.
// MODULE.05 -> GND.
// MODULE.06 -> MCU.RA0.
// MODULE.07 -> VDD.
// MODULE.08 -> VDD.
// MODULE.09 -> VDD.
// MODULE.10 -> VDD.
// MODULE.11 -> MCU.RB0.
// MODULE.12 -> MCU.RB1.
// MODULE.13 -> MCU.RB2.
// MODULE.14 -> MCU.RB3..
// MODULE.15 -> MCU.RA2.
// MODULE.16 -> GND.

// Module EA-DOGM081/16X.
// Jumpers.
// PSB - //.
// CAP1 - Open.
// VOUT - Close.

// Definitions.
// ST7036 Commands Port & Data Port.
#define ST7036_DATA                                        LATB
#define ST7036_E                                           do{LATAbits.LATA0 = 0b1;} while(0)
#define ST7036_nE                                          do{LATAbits.LATA0 = 0b0;} while(0)
#define ST7036_RS                                          do{LATAbits.LATA1 = 0b1;} while(0)
#define ST7036_nRS                                         do{LATAbits.LATA1 = 0b0;} while(0)
// ST7036 Instruction Table IS2=0, IS1=0.
#define ST7036_CLEAR_DISPLAY                               0x01
#define ST7036_RETURN_HOME                                 0x02
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_NOSHIFT      0x04
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_SHIFT_RIGHT  0x05
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT      0x06
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_SHIFT_LEFT   0x07
#define ST7036_DISPLAY_OFF                                 0x08
#define ST7036_DISPLAY_ON_CURSOR_OFF                       0x0C
#define ST7036_DISPLAY_ON_CURSOR_ON_NOBLINK                0x0E
#define ST7036_DISPLAY_ON_CURSOR_ON_BLINK                  0x0F
#define ST7036_DISPLAY_CURSOR_SHIFT_LEFT                   0x10
#define ST7036_DISPLAY_CURSOR_SHIFT_RIGHT                  0x14
#define ST7036_DISPLAY_DISPLAY_SHIFT_LEFT                  0x18
#define ST7036_DISPLAY_DISPLAY_SHIFT_RIGH                  0x1C
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8         0x20
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1     0x21
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS2     0x22
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8         0x28
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_DHFONT5x8       0x24
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_DHFONT5x8       0x2C
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8         0x30
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1     0x31
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS2     0x32
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_DHFONT5x8       0x34
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8         0x38
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_DHFONT5x8       0x3C
#define ST7036_SET_ICON_RAM_ADDRESS                        0X40
#define ST7036_SET_CGRAM_ADDRESS                           0x40
#define ST7036_DDRAM_ADDRESS_FIRST_LINE                    0x80
#define ST7036_DDRAM_ADDRESS_SECOND_LINE                   0xC0
// ST7036 Instruction Table IS2=0, IS1=1.
#define ST7036_BIAS_SET_1_5                                0x14
#define ST7036_BIAS_SET_1_5_3_LINE                         0x15
#define ST7036_BIAS_SET_1_4                                0x1C
#define ST7036_BIAS_SET_1_4_3_LINE                         0x1D
#define ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST        0x50
#define ST7036_POWER_ICON_ON_BOOST_OFF_NO_CONTRAST         0x58
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_0      0x54
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1      0x55
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_2      0x56
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_3      0x57
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_0       0x5C
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_1       0x5D
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_2       0x5E
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_3       0x5F
#define ST7036_FOLLOWER_CONTROL_OFF                        0x60
#define ST7036_FOLLOWER_CONTROL_ON_RAB_0                   0x68
#define ST7036_FOLLOWER_CONTROL_ON_RAB_1                   0x69
#define ST7036_FOLLOWER_CONTROL_ON_RAB_2                   0x6A
#define ST7036_FOLLOWER_CONTROL_ON_RAB_3                   0x6B
#define ST7036_FOLLOWER_CONTROL_ON_RAB_4                   0x6C
#define ST7036_FOLLOWER_CONTROL_ON_RAB_5                   0x6D
#define ST7036_FOLLOWER_CONTROL_ON_RAB_6                   0x6E
#define ST7036_FOLLOWER_CONTROL_ON_RAB_7                   0x6F
#define ST7036_CONTRAST_LSB_0                              0x70
#define ST7036_CONTRAST_LSB_1                              0x71
#define ST7036_CONTRAST_LSB_2                              0x72
#define ST7036_CONTRAST_LSB_3                              0x73
#define ST7036_CONTRAST_LSB_4                              0x74
#define ST7036_CONTRAST_LSB_5                              0x75
#define ST7036_CONTRAST_LSB_6                              0x76
#define ST7036_CONTRAST_LSB_7                              0x77
#define ST7036_CONTRAST_LSB_8                              0x78
#define ST7036_CONTRAST_LSB_9                              0x79
#define ST7036_CONTRAST_LSB_10                             0x7A
#define ST7036_CONTRAST_LSB_11                             0x7B
#define ST7036_CONTRAST_LSB_12                             0x7C
#define ST7036_CONTRAST_LSB_13                             0x7D
#define ST7036_CONTRAST_LSB_14                             0x7E
#define ST7036_CONTRAST_LSB_15                             0x7F
#define ST7036_INSTRUCTION_DELAY_US                        28
#define ST7036_CLEAR_DISPLAY_DELAY_MS                      1
#define ST7036_INITIALIZATION_DELAY_MS                     30
// ST7036 Instruction Table IS2=1, IS1=0.
#define ST7036_DOUBLE_HEIGHT_FONT_COM9_COM24               0x10
#define ST7036_DOUBLE_HEIGHT_FONT_COM1_COM16               0x11
// ASCII Characters.
#define ASCII_SPACE                                        0x20
// EA-DOGM Configuration.
#define DOGM_CONFIGURATION_FIRST_LINE                      ST7036_DDRAM_ADDRESS_FIRST_LINE
#define DOGM_CONFIGURATION_SECOND_LINE                     ST7036_DDRAM_ADDRESS_SECOND_LINE
#define DOGM_CONFIGURATION_CHARACTERS                      16
#define DOGM_BACKLIGHT_OFF                                 do{LATAbits.LATA2 = 0b0;} while(0)
#define DOGM_BACKLIGHT_ON                                  do{LATAbits.LATA2 = 0b1;} while(0)

// Function Prototype.
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Line);
void lcd_initialization(void);
void lcd_writeData(uint8_t u8Data);
void lcd_writeInstruction(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor);

// Strings & Custom Patterns.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8BatteryPattern[5][8] = {
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x11, 0x11, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f}
};

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
    PORTE = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000000;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    TRISE = 0b00000000;
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
    WPUE = 0b00000000;
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

    // ST7036 Initialization.
    lcd_initialization();

    // Write 5x8 Dots Custom Patterns in ST7036 CGRAM.
    uint8_t line, pattern;
    lcd_writeInstruction(ST7036_SET_CGRAM_ADDRESS);
    for(pattern=0; pattern<5; pattern++){
        for(line=0; line<8; line++){
            lcd_writeData(au8BatteryPattern[pattern][line]);
       }
    }

    // Display Strings.
    lcd_writeStringSetCursor(au8Tronix, DOGM_CONFIGURATION_FIRST_LINE);
    lcd_writeStringSetCursor(au8WWW, DOGM_CONFIGURATION_SECOND_LINE);

    DOGM_BACKLIGHT_ON;
    while(1){
        for(pattern=0; pattern<5; pattern++){
            lcd_writeInstruction(DOGM_CONFIGURATION_FIRST_LINE + 0x0f);
            lcd_writeData(pattern);
            __delay_ms(500);
        }
    }
}

// Functions.
void lcd_clearDisplay(void)
{
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_clearLine(uint8_t u8Line)
{
    uint8_t character = DOGM_CONFIGURATION_CHARACTERS;

    lcd_writeInstruction(u8Line);
    while(character--)
        lcd_writeData(ASCII_SPACE);
    lcd_writeInstruction(u8Line);
}

void lcd_initialization(void)
{
    __delay_ms(ST7036_INITIALIZATION_DELAY_MS);
    // Instruction Table IS2=0, IS1=1.
    lcd_writeInstruction(ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1);
    lcd_writeInstruction(ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1);
    lcd_writeInstruction(ST7036_BIAS_SET_1_4);
    lcd_writeInstruction(ST7036_CONTRAST_LSB_7);
    lcd_writeInstruction(ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST);
    lcd_writeInstruction(ST7036_FOLLOWER_CONTROL_ON_RAB_4);
    // Instruction Table IS2=0, IS1=0.
    lcd_writeInstruction(ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8);
    lcd_writeInstruction(ST7036_DISPLAY_ON_CURSOR_OFF);
    lcd_writeInstruction(ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT);
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_writeData(uint8_t u8Data)
{
    ST7036_RS;

    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= ((u8Data>>4) & 0x0f);
    ST7036_nE;
    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= (u8Data & 0x0f);
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeInstruction(uint8_t u8Data)
{
    ST7036_nRS;

    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= ((u8Data>>4) & 0x0f);
    ST7036_nE;
    ST7036_DATA &= 0xf0;
    ST7036_E;
    ST7036_DATA |= (u8Data & 0x0f);
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeString(const uint8_t * u8Data)
{
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor)
{
    lcd_writeInstruction(u8Cursor);
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}
```

## 5.PIC16F1773/6/8 Module EA-DOGM162 - 8-Bit - 5V.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1773/6/8 - Compile with XC8(v2.31).
// PIC16F1773/6/8 - @8MHz Internal Oscillator.

// Module - LCD Character - EA-DOGM162 - ST7036.
// 2 Lines, 8 Characters, 5V, 8 Bits.
// Strings & Patterns - v0.1 - 05/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MODULE.01 -> GND.
// MODULE.02 -> VDD.
// MODULE.03 -> GND.
// MODULE.04 -> MCU.RA1.
// MODULE.05 -> GND.
// MODULE.06 -> MCU.RA0.
// MODULE.07 -> MCU.RB0.
// MODULE.08 -> MCU.RB1.
// MODULE.09 -> MCU.RB2.
// MODULE.10 -> MCU.RB3.
// MODULE.11 -> MCU.RB4.
// MODULE.12 -> MCU.RB5.
// MODULE.13 -> MCU.RB6.
// MODULE.14 -> MCU.RB7.
// MODULE.15 -> MCU.RA2.
// MODULE.16 -> GND.

// Module EA-DOGM081/16X.
// Jumpers.
// PSB - //.
// CAP1 - Open.
// VOUT - Close.

// Definitions.
// ST7036 Commands Port & Data Port.
#define ST7036_DATA                                        LATB
#define ST7036_E                                           do{LATAbits.LATA0 = 0b1;} while(0)
#define ST7036_nE                                          do{LATAbits.LATA0 = 0b0;} while(0)
#define ST7036_RS                                          do{LATAbits.LATA1 = 0b1;} while(0)
#define ST7036_nRS                                         do{LATAbits.LATA1 = 0b0;} while(0)
// ST7036 Instruction Table IS2=0, IS1=0.
#define ST7036_CLEAR_DISPLAY                               0x01
#define ST7036_RETURN_HOME                                 0x02
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_NOSHIFT      0x04
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_SHIFT_RIGHT  0x05
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT      0x06
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_SHIFT_LEFT   0x07
#define ST7036_DISPLAY_OFF                                 0x08
#define ST7036_DISPLAY_ON_CURSOR_OFF                       0x0C
#define ST7036_DISPLAY_ON_CURSOR_ON_NOBLINK                0x0E
#define ST7036_DISPLAY_ON_CURSOR_ON_BLINK                  0x0F
#define ST7036_DISPLAY_CURSOR_SHIFT_LEFT                   0x10
#define ST7036_DISPLAY_CURSOR_SHIFT_RIGHT                  0x14
#define ST7036_DISPLAY_DISPLAY_SHIFT_LEFT                  0x18
#define ST7036_DISPLAY_DISPLAY_SHIFT_RIGH                  0x1C
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8         0x20
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1     0x21
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS2     0x22
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8         0x28
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_DHFONT5x8       0x24
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_DHFONT5x8       0x2C
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8         0x30
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1     0x31
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS2     0x32
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_DHFONT5x8       0x34
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8         0x38
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_DHFONT5x8       0x3C
#define ST7036_SET_ICON_RAM_ADDRESS                        0X40
#define ST7036_SET_CGRAM_ADDRESS                           0x40
#define ST7036_DDRAM_ADDRESS_FIRST_LINE                    0x80
#define ST7036_DDRAM_ADDRESS_SECOND_LINE                   0xC0
// ST7036 Instruction Table IS2=0, IS1=1.
#define ST7036_BIAS_SET_1_5                                0x14
#define ST7036_BIAS_SET_1_5_3_LINE                         0x15
#define ST7036_BIAS_SET_1_4                                0x1C
#define ST7036_BIAS_SET_1_4_3_LINE                         0x1D
#define ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST        0x50
#define ST7036_POWER_ICON_ON_BOOST_OFF_NO_CONTRAST         0x58
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_0      0x54
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1      0x55
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_2      0x56
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_3      0x57
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_0       0x5C
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_1       0x5D
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_2       0x5E
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_3       0x5F
#define ST7036_FOLLOWER_CONTROL_OFF                        0x60
#define ST7036_FOLLOWER_CONTROL_ON_RAB_0                   0x68
#define ST7036_FOLLOWER_CONTROL_ON_RAB_1                   0x69
#define ST7036_FOLLOWER_CONTROL_ON_RAB_2                   0x6A
#define ST7036_FOLLOWER_CONTROL_ON_RAB_3                   0x6B
#define ST7036_FOLLOWER_CONTROL_ON_RAB_4                   0x6C
#define ST7036_FOLLOWER_CONTROL_ON_RAB_5                   0x6D
#define ST7036_FOLLOWER_CONTROL_ON_RAB_6                   0x6E
#define ST7036_FOLLOWER_CONTROL_ON_RAB_7                   0x6F
#define ST7036_CONTRAST_LSB_0                              0x70
#define ST7036_CONTRAST_LSB_1                              0x71
#define ST7036_CONTRAST_LSB_2                              0x72
#define ST7036_CONTRAST_LSB_3                              0x73
#define ST7036_CONTRAST_LSB_4                              0x74
#define ST7036_CONTRAST_LSB_5                              0x75
#define ST7036_CONTRAST_LSB_6                              0x76
#define ST7036_CONTRAST_LSB_7                              0x77
#define ST7036_CONTRAST_LSB_8                              0x78
#define ST7036_CONTRAST_LSB_9                              0x79
#define ST7036_CONTRAST_LSB_10                             0x7A
#define ST7036_CONTRAST_LSB_11                             0x7B
#define ST7036_CONTRAST_LSB_12                             0x7C
#define ST7036_CONTRAST_LSB_13                             0x7D
#define ST7036_CONTRAST_LSB_14                             0x7E
#define ST7036_CONTRAST_LSB_15                             0x7F
#define ST7036_INSTRUCTION_DELAY_US                        28
#define ST7036_CLEAR_DISPLAY_DELAY_MS                      1
#define ST7036_INITIALIZATION_DELAY_MS                     30
// ST7036 Instruction Table IS2=1, IS1=0.
#define ST7036_DOUBLE_HEIGHT_FONT_COM9_COM24               0x10
#define ST7036_DOUBLE_HEIGHT_FONT_COM1_COM16               0x11
// ASCII Characters.
#define ASCII_SPACE                                        0x20
// EA-DOGM Configuration.
#define DOGM_CONFIGURATION_FIRST_LINE                      ST7036_DDRAM_ADDRESS_FIRST_LINE
#define DOGM_CONFIGURATION_SECOND_LINE                     ST7036_DDRAM_ADDRESS_SECOND_LINE
#define DOGM_CONFIGURATION_CHARACTERS                      16
#define DOGM_BACKLIGHT_OFF                                 do{LATAbits.LATA2 = 0b0;} while(0)
#define DOGM_BACKLIGHT_ON                                  do{LATAbits.LATA2 = 0b1;} while(0)

// Function Prototype.
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Line);
void lcd_initialization(void);
void lcd_writeData(uint8_t u8Data);
void lcd_writeInstruction(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor);

// Strings & Custom Patterns.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8BatteryPattern[5][8] = {
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x11, 0x11, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f}
};

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
    PORTE = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000000;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    TRISE = 0b00000000;
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
    WPUE = 0b00000000;
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

    // ST7036 Initialization.
    lcd_initialization();

    // Write 5x8 Dots Custom Patterns in ST7036 CGRAM.
    uint8_t line, pattern;
    lcd_writeInstruction(ST7036_SET_CGRAM_ADDRESS);
    for(pattern=0; pattern<5; pattern++){
        for(line=0; line<8; line++){
            lcd_writeData(au8BatteryPattern[pattern][line]);
       }
    }

    // Display Strings.
    lcd_writeStringSetCursor(au8Tronix, DOGM_CONFIGURATION_FIRST_LINE);
    lcd_writeStringSetCursor(au8WWW, DOGM_CONFIGURATION_SECOND_LINE);

    DOGM_BACKLIGHT_ON;
    while(1){
        for(pattern=0; pattern<5; pattern++){
            lcd_writeInstruction(DOGM_CONFIGURATION_FIRST_LINE + 0x0f);
            lcd_writeData(pattern);
            __delay_ms(500);
        }
    }
}

// Functions.
void lcd_clearDisplay(void)
{
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_clearLine(uint8_t u8Line)
{
    uint8_t character = DOGM_CONFIGURATION_CHARACTERS;

    lcd_writeInstruction(u8Line);
    while(character--)
        lcd_writeData(ASCII_SPACE);
    lcd_writeInstruction(u8Line);
}

void lcd_initialization(void)
{
    __delay_ms(ST7036_INITIALIZATION_DELAY_MS);
    // Instruction Table IS2=0, IS1=1.
    lcd_writeInstruction(ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1);
    lcd_writeInstruction(ST7036_BIAS_SET_1_4);
    lcd_writeInstruction(ST7036_CONTRAST_LSB_7);
    lcd_writeInstruction(ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST);
    lcd_writeInstruction(ST7036_FOLLOWER_CONTROL_ON_RAB_4);
    // Instruction Table IS2=0, IS1=0.
    lcd_writeInstruction(ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8);
    lcd_writeInstruction(ST7036_DISPLAY_ON_CURSOR_OFF);
    lcd_writeInstruction(ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT);
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_writeData(uint8_t u8Data)
{
    ST7036_RS;
    ST7036_E;
    ST7036_DATA = u8Data;
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeInstruction(uint8_t u8Data)
{
    ST7036_nRS;
    ST7036_E;
    ST7036_DATA = u8Data;
    ST7036_nE;
    __delay_us(ST7036_INSTRUCTION_DELAY_US);
}

void lcd_writeString(const uint8_t * u8Data)
{
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor)
{
    lcd_writeInstruction(u8Cursor);
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}
```

## 6.PIC16F1773/6/8 Module EA-DOGM162 - SPI - 5V.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1773/6/8 - Compile with XC8(v2.31).
// PIC16F1773/6/8 - @8MHz Internal Oscillator.

// Module - LCD Character - EA-DOGM162 - ST7036.
// 2 Lines, 8 Characters, 5V, SPI.
// Strings & Patterns - v0.1 - 05/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Open.
// SCL - Open.
// VEE - Open.
// BCKL - Open.
// VCAP - Open.
// SPI/I2C - Open.

// Pinout.
// MODULE.01 -> GND.
// MODULE.02 -> VDD.
// MODULE.03 -> MCU.RA0.
// MODULE.04 -> MCU.RA1.
// MODULE.05 -> GND.
// MODULE.06 -> VDD.
// MODULE.07 -> VDD.
// MODULE.08 -> VDD.
// MODULE.09 -> VDD.
// MODULE.10 -> VDD.
// MODULE.11 -> VDD.
// MODULE.12 -> VDD.
// MODULE.13 -> MCU.RC3.
// MODULE.14 -> MCU.RC5.
// MODULE.15 -> MCU.RA2.
// MODULE.16 -> GND.

// Module EA-DOGM081/16X.
// Jumpers.
// PSB - SPI.
// CAP1 - Open.
// VOUT - Close.

// Definitions.
// SPI.
#define SPI_CS                                             do{LATAbits.LATA0 = 0b1;} while(0)
#define SPI_nCS                                            do{LATAbits.LATA0 = 0b0;} while(0)
// ST7036 Commands Port.
#define ST7036_RS                                          do{LATAbits.LATA1 = 0b1;} while(0)
#define ST7036_nRS                                         do{LATAbits.LATA1 = 0b0;} while(0)
// ST7036 Instruction Table IS2=0, IS1=0.
#define ST7036_CLEAR_DISPLAY                               0x01
#define ST7036_RETURN_HOME                                 0x02
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_NOSHIFT      0x04
#define ST7036_ENTRY_MODE_SET_DDRAM_DECREMENT_SHIFT_RIGHT  0x05
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT      0x06
#define ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_SHIFT_LEFT   0x07
#define ST7036_DISPLAY_OFF                                 0x08
#define ST7036_DISPLAY_ON_CURSOR_OFF                       0x0C
#define ST7036_DISPLAY_ON_CURSOR_ON_NOBLINK                0x0E
#define ST7036_DISPLAY_ON_CURSOR_ON_BLINK                  0x0F
#define ST7036_DISPLAY_CURSOR_SHIFT_LEFT                   0x10
#define ST7036_DISPLAY_CURSOR_SHIFT_RIGHT                  0x14
#define ST7036_DISPLAY_DISPLAY_SHIFT_LEFT                  0x18
#define ST7036_DISPLAY_DISPLAY_SHIFT_RIGH                  0x1C
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8         0x20
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS1     0x21
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_FONT5x8_IS2     0x22
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_FONT5x8         0x28
#define ST7036_FUNCTION_SET_4_BIT_ONE_LINE_DHFONT5x8       0x24
#define ST7036_FUNCTION_SET_4_BIT_TWO_LINE_DHFONT5x8       0x2C
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8         0x30
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1     0x31
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS2     0x32
#define ST7036_FUNCTION_SET_8_BIT_ONE_LINE_DHFONT5x8       0x34
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8         0x38
#define ST7036_FUNCTION_SET_8_BIT_TWO_LINE_DHFONT5x8       0x3C
#define ST7036_SET_ICON_RAM_ADDRESS                        0X40
#define ST7036_SET_CGRAM_ADDRESS                           0x40
#define ST7036_DDRAM_ADDRESS_FIRST_LINE                    0x80
#define ST7036_DDRAM_ADDRESS_SECOND_LINE                   0xC0
// ST7036 Instruction Table IS2=0, IS1=1.
#define ST7036_BIAS_SET_1_5                                0x14
#define ST7036_BIAS_SET_1_5_3_LINE                         0x15
#define ST7036_BIAS_SET_1_4                                0x1C
#define ST7036_BIAS_SET_1_4_3_LINE                         0x1D
#define ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST        0x50
#define ST7036_POWER_ICON_ON_BOOST_OFF_NO_CONTRAST         0x58
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_0      0x54
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_1      0x55
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_2      0x56
#define ST7036_POWER_ICON_OFF_BOOST_ON_CONTRAST_MSB_3      0x57
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_0       0x5C
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_1       0x5D
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_2       0x5E
#define ST7036_POWER_ICON_ON_BOOST_ON_CONTRAST_MSB_3       0x5F
#define ST7036_FOLLOWER_CONTROL_OFF                        0x60
#define ST7036_FOLLOWER_CONTROL_ON_RAB_0                   0x68
#define ST7036_FOLLOWER_CONTROL_ON_RAB_1                   0x69
#define ST7036_FOLLOWER_CONTROL_ON_RAB_2                   0x6A
#define ST7036_FOLLOWER_CONTROL_ON_RAB_3                   0x6B
#define ST7036_FOLLOWER_CONTROL_ON_RAB_4                   0x6C
#define ST7036_FOLLOWER_CONTROL_ON_RAB_5                   0x6D
#define ST7036_FOLLOWER_CONTROL_ON_RAB_6                   0x6E
#define ST7036_FOLLOWER_CONTROL_ON_RAB_7                   0x6F
#define ST7036_CONTRAST_LSB_0                              0x70
#define ST7036_CONTRAST_LSB_1                              0x71
#define ST7036_CONTRAST_LSB_2                              0x72
#define ST7036_CONTRAST_LSB_3                              0x73
#define ST7036_CONTRAST_LSB_4                              0x74
#define ST7036_CONTRAST_LSB_5                              0x75
#define ST7036_CONTRAST_LSB_6                              0x76
#define ST7036_CONTRAST_LSB_7                              0x77
#define ST7036_CONTRAST_LSB_8                              0x78
#define ST7036_CONTRAST_LSB_9                              0x79
#define ST7036_CONTRAST_LSB_10                             0x7A
#define ST7036_CONTRAST_LSB_11                             0x7B
#define ST7036_CONTRAST_LSB_12                             0x7C
#define ST7036_CONTRAST_LSB_13                             0x7D
#define ST7036_CONTRAST_LSB_14                             0x7E
#define ST7036_CONTRAST_LSB_15                             0x7F
#define ST7036_INSTRUCTION_DELAY_US                        28
#define ST7036_CLEAR_DISPLAY_DELAY_MS                      1
#define ST7036_INITIALIZATION_DELAY_MS                     30
// ST7036 Instruction Table IS2=1, IS1=0.
#define ST7036_DOUBLE_HEIGHT_FONT_COM9_COM24               0x10
#define ST7036_DOUBLE_HEIGHT_FONT_COM1_COM16               0x11
// ASCII Characters.
#define ASCII_SPACE                                        0x20
// EA-DOGM Configuration.
#define DOGM_CONFIGURATION_FIRST_LINE                      ST7036_DDRAM_ADDRESS_FIRST_LINE
#define DOGM_CONFIGURATION_SECOND_LINE                     ST7036_DDRAM_ADDRESS_SECOND_LINE
#define DOGM_CONFIGURATION_CHARACTERS                      16
#define DOGM_BACKLIGHT_OFF                                 do{LATAbits.LATA2 = 0b0;} while(0)
#define DOGM_BACKLIGHT_ON                                  do{LATAbits.LATA2 = 0b1;} while(0)

// Function Prototype.
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Line);
void lcd_initialization(void);
void lcd_writeData(uint8_t u8Data);
void lcd_writeInstruction(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor);

// Strings & Custom Patterns.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8BatteryPattern[5][8] = {
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x11, 0x11, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x11, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x11, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1b, 0x11, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f},
    {0x0e, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f, 0x1f}
};

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
    PORTE = 0b00000000;
    // TRIS Data Direction.
    TRISA = 0b00000000;
    TRISB = 0b00000000;
    TRISC = 0b00000000;
    TRISE = 0b00000000;
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
    WPUE = 0b00000000;
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
    // PPS Outputs.
    RC3PPSbits.RC3PPS = 0x21;    // RC3 - MSSP.SCK.
    RC5PPSbits.RC5PPS = 0x23;    // RC5 - MSSP.SDO.
    PPSLOCK = 0x55;
    PPSLOCK = 0xAA;
    PPSLOCKbits.PPSLOCKED = 0b1;

    // SPI Mater Settings.
    // Mode 0 - Fsck = Fosc/16.
    SSP1BUF = 0;
    SSP1ADD = 0;
    SSP1MSK = 0;
    SSP1STAT = 0x40;
    SSP1CON1 = 0x01;
    SSP1CON2 = 0x00;
    SSP1CON3 = 0x00;
    // SPI Enable.
    SSP1CON1bits.SSPEN = 0b1;

    // ST7036 Initialization.
    lcd_initialization();

    // Write 5x8 Dots Custom Patterns in ST7036 CGRAM.
    uint8_t line, pattern;
    lcd_writeInstruction(ST7036_SET_CGRAM_ADDRESS);
    for(pattern=0; pattern<5; pattern++){
        for(line=0; line<8; line++){
            lcd_writeData(au8BatteryPattern[pattern][line]);
       }
    }

    // Display Strings.
    lcd_writeStringSetCursor(au8Tronix, DOGM_CONFIGURATION_FIRST_LINE);
    lcd_writeStringSetCursor(au8WWW, DOGM_CONFIGURATION_SECOND_LINE);

    DOGM_BACKLIGHT_ON;
    while(1){
        for(pattern=0; pattern<5; pattern++){
            lcd_writeInstruction(DOGM_CONFIGURATION_FIRST_LINE + 0x0f);
            lcd_writeData(pattern);
            __delay_ms(500);
        }
    }
}

// Functions.
void lcd_clearDisplay(void)
{
    lcd_writeInstruction(ST7036_CLEAR_DISPLAY);
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_clearLine(uint8_t u8Line)
{
    uint8_t character = DOGM_CONFIGURATION_CHARACTERS;

    lcd_writeInstruction(u8Line);
    while(character--)
        lcd_writeData(ASCII_SPACE);
    lcd_writeInstruction(u8Line);
}

void lcd_initialization(void)
{
    uint8_t u8Dummy;

    SPI_nCS;
    ST7036_nRS;
    __delay_ms(ST7036_INITIALIZATION_DELAY_MS);
    // Instruction Table IS2=0, IS1=1.
    SSPBUF = ST7036_FUNCTION_SET_8_BIT_ONE_LINE_FONT5x8_IS1;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_BIAS_SET_1_4;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_CONTRAST_LSB_7;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_POWER_ICON_OFF_BOOST_OFF_NO_CONTRAST;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_FOLLOWER_CONTROL_ON_RAB_4;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    // Instruction Table IS2=0, IS1=0.
    SSPBUF = ST7036_FUNCTION_SET_8_BIT_TWO_LINE_FONT5x8;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_DISPLAY_ON_CURSOR_OFF;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_ENTRY_MODE_SET_DDRAM_INCREMENT_NOSHIFT;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SSPBUF = ST7036_CLEAR_DISPLAY;
    while(!SSPSTATbits.BF){};
    u8Dummy = SSPBUF;
    SPI_CS;
    __delay_ms(ST7036_CLEAR_DISPLAY_DELAY_MS);
}

void lcd_writeData(uint8_t u8Data)
{
    uint8_t u8Dummy;

    SPI_nCS;
    ST7036_RS;
    SSP1BUF = (u8Data);
    while(!SSP1STATbits.BF){};
    u8Dummy = SSP1BUF;
    SPI_CS;
}

void lcd_writeInstruction(uint8_t u8Data)
{
    uint8_t u8Dummy;

    SPI_nCS;
    ST7036_nRS;
    SSP1BUF = (u8Data);
    while(!SSP1STATbits.BF){};
    u8Dummy = SSP1BUF;
    SPI_CS;
}

void lcd_writeString(const uint8_t * u8Data)
{
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Cursor)
{
    lcd_writeInstruction(u8Cursor);
    while(*u8Data != '\0')
        lcd_writeData(*u8Data++);
}
```

---

DISCLAIMER: THIS CODE IS PROVIDED WITHOUT ANY WARRANTY OR GUARANTEES.
USERS MAY USE THIS CODE FOR DEVELOPMENT AND EXAMPLE PURPOSES ONLY.
AUTHORS ARE NOT RESPONSIBLE FOR ANY ERRORS, OMISSIONS, OR DAMAGES THAT COULD
RESULT FROM USING THIS FIRMWARE IN WHOLE OR IN PART.