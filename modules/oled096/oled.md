# OLED 0.96.

## Contents.

- [1.PIC16F1773/6/7/8/9 Module I2C OLED 0.96 - Page Mode.](#1pic16f17736789-module-i2c-oled-096---page-mode)
- [2.PIC16F1773/6/7/8/9 Module I2C OLED 0.96 - Horizontal Mode.](#2pic16f17736789-module-i2c-oled-096---horizontal-mode)

## 1.PIC16F1773/6/7/8/9 Module I2C OLED 0.96 - Page Mode.

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

// Module I2C - LCD Graphical - OLED0.96 128x64 - SSD1306.
// SSD1306 - Memory Page Addressing Mode.
// Strings & Patterns - v0.2 - 06/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Close.
// SCL - Close.
// VCAP - Open.
// VEE - Open.
// BCKL - Open.
// SPI/I2C - I2C.

// Module I2C - OLED0.96.
// Jumpers.
// SDA - Open.
// SCL - Open.

// Definitions.
// I2C.
#define I2C_READ                             0b1
#define I2C_WRITE                            0b0
#define I2C_FSCL_HZ                          400000
#define I2C_ADD                              ((_XTAL_FREQ/(4*I2C_FSCL_HZ))-1)
// SSD1306 Address Select Bits.
#define SSD1306_ADDRESS_78                   0x78
#define SSD1306_ADDRESS_7A                   0x7A
// SSD1306 Configuration Registers.
#define SSD1306_PAGE_ADDRESS                 0x22
#define SSD1306_PAGE_ADDRESS_0               0xB0
#define SSD1306_PAGE_ADDRESS_1               0xB1
#define SSD1306_PAGE_ADDRESS_2               0xB2
#define SSD1306_PAGE_ADDRESS_3               0xB3
#define SSD1306_PAGE_ADDRESS_4               0xB4
#define SSD1306_PAGE_ADDRESS_5               0xB5
#define SSD1306_PAGE_ADDRESS_6               0xB6
#define SSD1306_PAGE_ADDRESS_7               0xB7
#define	SSD1306_COLUMN_ADDRESS               0x21
#define SSD1306_MEMORY_ADDRESS_MODE          0x20
#define SSD1306_MEMORY_ADDRESS_HORIZONTAL    0x00
#define SSD1306_MEMORY_ADDRESS_VERTICAL      0x01
#define SSD1306_MEMORY_ADDRESS_PAGE          0x02
#define	SSD1306_COLUMN_ADDRESS_PAGE_LSB      0x00
#define	SSD1306_COLUMN_ADDRESS_PAGE_MSB      0x10
#define SSD1306_DISPLAY_CONTRAST             0x81
#define SSD1306_DISPLAY_ALL_POINT_RAM        0xA4
#define SSD1306_DISPLAY_ALL_POINT_ON         0xA5
#define SSD1306_DISPLAY_NORMAL               0xA6
#define SSD1306_DISPLAY_INVERSE              0xA7
#define SSD1306_DISPLAY_OFF                  0xAE
#define SSD1306_DISPLAY_ON                   0xAF
#define SSD1306_START_LINE                   0x40
#define SSD1306_ADDRESS_COLUMN_NORMAL        0xA0
#define SSD1306_ADDRESS_COLUMN_REVERSE       0xA1
#define SSD1306_MULTIPLEX_RATIO              0xA8
#define SSD1306_COM_OUTPUT_NORMAL            0xC0
#define SSD1306_COM_OUTPUT_REVERSE           0xC8
#define SSD1306_OFFSET                       0xD3
#define	SSD1306_COM_PIN                      0xDA
#define	SSD1306_COM_PIN_SEQUENTIAL           0x02
#define	SSD1306_COM_PIN_ALTERNATIVE          0x12
#define	SSD1306_COM_PIN_DISABLE              0x20
#define	SSD1306_COM_PIN_ENABLE               0x22
#define SSD1306_CLOCK_RATIO                  0xD5
#define SSD1306_PRECHARGE                    0xD9
#define SSD1306_VCOMH_DESELECT_LEVEL         0xDB
#define SSD1306_NOOP                         0xE3
#define SSD1306_CHARGE_PUMP                  0x8D
#define SSD1306_CHARGE_PUMP_ON               0x14
#define SSD1306_CHARGE_PUMP_OFF              0x10
#define SSD1306_COMMAND                      0x00
#define SSD1306_DATA                         0x40
#define SSD1306_INITIALIZATION_DELAY_MS      400
// OLED 0.96 Configuration.
#define OLED096_CONFIGURATION_I2C_ADDRESS    SSD1306_ADDRESS_78
#define OLED096_CONFIGURATION_COLUMN         128
#define OLED096_CONFIGURATION_ROW            64
#define OLED096_CONFIGURATION_PAGE           8
#define OLED096_CONFIGURATION_LINE_1         SSD1306_PAGE_ADDRESS_0
#define OLED096_CONFIGURATION_LINE_2         SSD1306_PAGE_ADDRESS_1
#define OLED096_CONFIGURATION_LINE_3         SSD1306_PAGE_ADDRESS_2
#define OLED096_CONFIGURATION_LINE_4         SSD1306_PAGE_ADDRESS_3
#define OLED096_CONFIGURATION_LINE_5         SSD1306_PAGE_ADDRESS_4
#define OLED096_CONFIGURATION_LINE_6         SSD1306_PAGE_ADDRESS_5
#define OLED096_CONFIGURATION_LINE_7         SSD1306_PAGE_ADDRESS_6
#define OLED096_CONFIGURATION_LINE_8         SSD1306_PAGE_ADDRESS_7
// Battery Patterns.
#define BATTERY_EMPTY                        0
#define BATTERY_1_3                          1
#define BATTERY_2_3                          2
#define BATTERY_FULL                         3

// Functions Prototype.
void i2c_restart(void);
void i2c_start(void);
void i2c_stop(void);
void i2c_write(uint8_t u8Data);
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Page);
void lcd_displayContrast(uint8_t u8Data);
void lcd_initialize(void);
void lcd_setCursor(uint8_t u8Page, uint8_t u8MSB, uint8_t u8LSB);
void lcd_writeBattery(uint8_t u8Battery);
void lcd_writeCharacter(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Page, uint8_t u8MSB, uint8_t u8LSB);
void ssd1306_writeFont(uint8_t u8Data);

// Strings.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8Open[] = "OPEN SMART";
const uint8_t au8Oled[] = "OLED 0.96 - SSD1306";

// Font 5x7.
const uint8_t au8Font5x7[96][5] = {
    {0x00, 0x00, 0x00, 0x00, 0x00}, // Space
    {0x00, 0x00, 0x5F, 0x00, 0x00}, // !
    {0x00, 0x07, 0x00, 0x07, 0x00}, // "
    {0x14, 0x7F, 0x14, 0x7F, 0x14}, // #
    {0x24, 0x2A, 0x7f, 0x2A, 0x12}, // $
    {0x23, 0x13, 0x08, 0x64, 0x62}, // %
    {0x36, 0x49, 0x55, 0x22, 0x50}, // &
    {0x00, 0x00, 0x03, 0x00, 0x00}, // '
    {0x00, 0x1C, 0x22, 0x41, 0x00}, // (
    {0x00, 0x41, 0x22, 0x1C, 0x00}, // )
    {0x14, 0x08, 0x3E, 0x08, 0x14}, // *
    {0x08, 0x08, 0x3E, 0x08, 0x08}, // +
    {0x00, 0x50, 0x30, 0x00, 0x00}, // ,
    {0x08, 0x08, 0x08, 0x08, 0x08}, // -
    {0x00, 0x60, 0x60, 0x00, 0x00}, // .
    {0x20, 0x10, 0x08, 0x04, 0x02}, // /
    {0x3E, 0x51, 0x49, 0x45, 0x3E}, // 0
    {0x00, 0x42, 0x7F, 0x40, 0x00}, // 1
    {0x42, 0x61, 0x51, 0x49, 0x46}, // 2
    {0x21, 0x41, 0x45, 0x4B, 0x31}, // 3
    {0x18, 0x14, 0x12, 0x7F, 0x10}, // 4
    {0x27, 0x45, 0x45, 0x45, 0x39}, // 5
    {0x3C, 0x4A, 0x49, 0x49, 0x30}, // 6
    {0x03, 0x71, 0x09, 0x05, 0x03}, // 7
    {0x36, 0x49, 0x49, 0x49, 0x36}, // 8
    {0x06, 0x49, 0x49, 0x29, 0x1E}, // 9
    {0x00, 0x36, 0x36, 0x00, 0x00}, // :
    {0x00, 0x56, 0x36, 0x00, 0x00}, // ;
    {0x08, 0x14, 0x22, 0x41, 0x00}, // <
    {0x14, 0x14, 0x14, 0x14, 0x14}, // =
    {0x00, 0x41, 0x22, 0x14, 0x08}, // >
    {0x02, 0x01, 0x51, 0x09, 0x06}, // ?
    {0x32, 0x49, 0x79, 0x41, 0x3E}, // @
    {0x7E, 0x11, 0x11, 0x11, 0x7E}, // A
    {0x7F, 0x49, 0x49, 0x49, 0x36}, // B
    {0x3E, 0x41, 0x41, 0x41, 0x22}, // C
    {0x7F, 0x41, 0x41, 0x41, 0x3E}, // D
    {0x7F, 0x49, 0x49, 0x49, 0x41}, // E
    {0x7F, 0x09, 0x09, 0x09, 0x01}, // F
    {0x3E, 0x41, 0x41, 0x49, 0x7A}, // G
    {0x7F, 0x08, 0x08, 0x08, 0x7F}, // H
    {0x00, 0x41, 0x7F, 0x41, 0x00}, // I
    {0x20, 0x40, 0x41, 0x3F, 0x01}, // J
    {0x7F, 0x08, 0x14, 0x22, 0x41}, // K
    {0x7F, 0x40, 0x40, 0x40, 0x40}, // L
    {0x7F, 0x02, 0x0C, 0x02, 0x7F}, // M
    {0x7F, 0x04, 0x08, 0x10, 0x7F}, // N
    {0x3E, 0x41, 0x41, 0x41, 0x3E}, // O
    {0x7F, 0x09, 0x09, 0x09, 0x06}, // P
    {0x3E, 0x41, 0x51, 0x21, 0x5E}, // Q
    {0x7F, 0x09, 0x19, 0x29, 0x46}, // R
    {0x26, 0x49, 0x49, 0x49, 0x32}, // S
    {0x01, 0x01, 0x7F, 0x01, 0x01}, // T
    {0x3F, 0x40, 0x40, 0x40, 0x3F}, // U
    {0x1F, 0x20, 0x40, 0x20, 0x1F}, // V
    {0x3F, 0x40, 0x38, 0x40, 0x3F}, // W
    {0x63, 0x14, 0x08, 0x14, 0x63}, // X
    {0x07, 0x08, 0x70, 0x08, 0x07}, // Y
    {0x61, 0x51, 0x49, 0x45, 0x43}, // Z
    {0x00, 0x7F, 0x41, 0x41, 0x00}, // [
    {0x02, 0x04, 0x08, 0x10, 0x20}, //
    {0x00, 0x41, 0x41, 0x7F, 0x00}, // ]
    {0x04, 0x02, 0x01, 0x02, 0x04}, // ^
    {0x40, 0x40, 0x40, 0x40, 0x40}, // _
    {0x00, 0x01, 0x02, 0x04, 0x00}, // '
    {0x20, 0x54, 0x54, 0x54, 0x78}, // a
    {0x7F, 0x48, 0x44, 0x44, 0x38}, // b
    {0x38, 0x44, 0x44, 0x44, 0x20}, // c
    {0x38, 0x44, 0x44, 0x48, 0x7F}, // d
    {0x38, 0x54, 0x54, 0x54, 0x18}, // e
    {0x08, 0x7E, 0x09, 0x01, 0x02}, // f
    {0x08, 0x54, 0x54, 0x54, 0x3C}, // g
    {0x7F, 0x08, 0x04, 0x04, 0x78}, // h
    {0x00, 0x48, 0x7D, 0x40, 0x00}, // i
    {0x20, 0x40, 0x44, 0x3D, 0x00}, // j
    {0x7F, 0x10, 0x28, 0x44, 0x00}, // k
    {0x00, 0x41, 0x7F, 0x40, 0x00}, // l
    {0x7C, 0x04, 0x78, 0x04, 0x78}, // m
    {0x7C, 0x08, 0x04, 0x04, 0x78}, // n
    {0x38, 0x44, 0x44, 0x44, 0x38}, // o
    {0x7C, 0x14, 0x14, 0x14, 0x08}, // p
    {0x08, 0x14, 0x14, 0x18, 0x7C}, // q
    {0x7C, 0x08, 0x04, 0x04, 0x08}, // r
    {0x48, 0x54, 0x54, 0x54, 0x20}, // s
    {0x04, 0x3F, 0x44, 0x40, 0x20}, // t
    {0x3C, 0x40, 0x40, 0x20, 0x7C}, // u
    {0x1C, 0x20, 0x40, 0x20, 0x1C}, // v
    {0x3C, 0x40, 0x30, 0x40, 0x3C}, // w
    {0x44, 0x28, 0x10, 0x28, 0x44}, // x
    {0x0C, 0x50, 0x50, 0x50, 0x3C}, // y
    {0x44, 0x64, 0x54, 0x4C, 0x44}, // z
    {0x00, 0x08, 0x36, 0x41, 0x00}, // {
    {0x00, 0x00, 0x7F, 0x00, 0x00}, // |
    {0x00, 0x41, 0x36, 0x08, 0x00}, // }
    {0x10, 0x08, 0x08, 0x10, 0x08}, // ~
    {0x06, 0x09, 0x09, 0x06, 0x00}  // °
};

// Battery Patterns.
const uint8_t au8Battery[5][14] = {
    {0x7e, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x7e, 0x3c, 0x3c}, // Battery Empty.
    {0x7e, 0x42, 0x5a, 0x5a, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x7e, 0x3c, 0x3c}, // Battery 1/3.
    {0x7e, 0x42, 0x5a, 0x5a, 0x42, 0x5a, 0x5a, 0x42, 0x42, 0x42, 0x42, 0x7e, 0x3c, 0x3c}, // Battery 2/3.
    {0x7e, 0x42, 0x5a, 0x5a, 0x42, 0x5a, 0x5a, 0x42, 0x5a, 0x5a, 0x42, 0x7e, 0x3c, 0x3c}  // Battery Full.
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
    TRISC = 0b00011000;
    TRISE = 0b00000000;
    // WPU Disable.
    OPTION_REGbits.nWPUEN = 0b1;
    // LATCH Outputs.
    LATA = 0b00000000;
    LATB = 0b00000000;
    LATC = 0b00011000;
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
    // PPS Inputs.
    SSPDATPPSbits.SSPDATPPS = 0x14;  // RC4 - MSSP.SDA.
    // PPS Outputs.
    RC3PPSbits.RC3PPS = 0x21;        // RC3 - MSSP.SCL.
    RC4PPSbits.RC4PPS = 0x22;        // RC4 - MSSP.SDA.
    PPSLOCK = 0x55;
    PPSLOCK = 0xAA;
    PPSLOCKbits.PPSLOCKED = 0b1;

    // I2C Master Settings.
    SSP1BUF = 0;
    SSP1ADD = I2C_ADD;
    SSP1STAT = 0x00;
    SSP1CON1 = 0x08;
    SSP1CON2 = 0x00;
    SSP1CON3 = 0x00;
     // I2C Enable.
    SSP1CON1bits.SSPEN = 0b1;

    // ST7036 Initialization.
    lcd_initialize();

    // Display Strings & Pattern.
    lcd_clearDisplay();
    lcd_writeStringSetCursor(au8Tronix, OLED096_CONFIGURATION_LINE_1, 2, 2);
    lcd_writeStringSetCursor(au8WWW, OLED096_CONFIGURATION_LINE_2, 1, 7);
    lcd_writeStringSetCursor(au8Open, OLED096_CONFIGURATION_LINE_4, 2, 2);
    lcd_writeStringSetCursor(au8Oled, OLED096_CONFIGURATION_LINE_5, 0, 8);
    lcd_setCursor(OLED096_CONFIGURATION_LINE_1, 15, 0);
    lcd_writeBattery(BATTERY_FULL);
    __delay_ms(3000);

    uint8_t u8Column, u8Page;
    while(1){
        // Checkerboard Pattern.
        for(u8Page=2; u8Page<OLED096_CONFIGURATION_PAGE; u8Page++){
          lcd_setCursor(OLED096_CONFIGURATION_LINE_1 + u8Page, 0, 0);
          for(u8Column=0; u8Column<(OLED096_CONFIGURATION_COLUMN/2); u8Column++){
              lcd_writeCharacter(0x55);
              lcd_writeCharacter(0xaa);
          }
        }
        __delay_ms(1000);

        // Full Pattern.
        for(u8Page=2; u8Page<OLED096_CONFIGURATION_PAGE; u8Page++){
          lcd_setCursor(OLED096_CONFIGURATION_LINE_1 + u8Page, 0, 0);
          for(u8Column=0; u8Column<OLED096_CONFIGURATION_COLUMN; u8Column++){
              lcd_writeCharacter(0xff);
          }
        }
        __delay_ms(1000);
    }
}

// Functions.
void i2c_restart(void)
{
    while(SSP1CON2 & 0x1F){};
    SSP1CON2bits.RSEN = 0b1;
    while(SSP1CON2bits.RSEN){};
}

void i2c_start(void)
{
    while(SSP1CON2 & 0x1F){};
    SSP1CON2bits.SEN = 0b1;
    while(SSP1CON2bits.SEN){};
}

void i2c_stop(void)
{
    while(SSP1CON2 & 0x1F){};
    SSP1CON2bits.PEN = 0b1;
    while(SSP1CON2bits.PEN){};
}

void i2c_write(uint8_t u8Data)
{
    SSP1BUF = u8Data;
    while(SSP1STATbits.BF){};
}

void lcd_clearDisplay(void)
{
    uint8_t u8Page;
    uint8_t u8Column = OLED096_CONFIGURATION_COLUMN;

    i2c_start();
    for(u8Page=0; u8Page<OLED096_CONFIGURATION_PAGE; u8Page++){
        i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
        i2c_write(SSD1306_COMMAND);
        i2c_write(SSD1306_PAGE_ADDRESS_0 + u8Page);
        i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_LSB);
        i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_MSB);
        i2c_restart();
        i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
        i2c_write(SSD1306_DATA);
        while(u8Column--){
            i2c_write(0x00);
        }
        i2c_restart();
    }
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_PAGE_ADDRESS_0);
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_LSB);
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_MSB);
    i2c_stop();
}

void lcd_clearLine(uint8_t u8Page)
{
    uint8_t u8Column = OLED096_CONFIGURATION_COLUMN;

    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(u8Page);
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_LSB);
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_MSB);
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    while(u8Column--){
        i2c_write(0x00);
    }
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(u8Page);
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_LSB);
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_MSB);
    i2c_stop();
}

void lcd_displayContrast(uint8_t u8Data)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_DISPLAY_CONTRAST);
    i2c_write(u8Data);
    i2c_stop();
}

void lcd_initialize(void)
{
    __delay_ms(SSD1306_INITIALIZATION_DELAY_MS);

    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    // Hardware.
    // Set display start line (Default 0x40).
    i2c_write(SSD1306_START_LINE);
    // Column address 127 is mapped to SEG0.
    i2c_write(SSD1306_ADDRESS_COLUMN_REVERSE);
    // Set Multiplex Ratio 0x00 - 0x3F (Default 0x3F).
    i2c_write(SSD1306_MULTIPLEX_RATIO);
    i2c_write(0x3f);
    // Set Remapped Mode COM[MUX_RATiO-1] to COM0.
    i2c_write(SSD1306_COM_OUTPUT_REVERSE);
    // Set Display Offset (Default 0x00);
    i2c_write(SSD1306_OFFSET);
    i2c_write(0x00);
    // Set COM Pins (Default 0x02)
    i2c_write(SSD1306_COM_PIN);
    i2c_write(SSD1306_COM_PIN_ALTERNATIVE);
    // Timing.
    // Set Display Clock Divide Ratio / Oscillator (Default 0x80).
    i2c_write(SSD1306_CLOCK_RATIO);
    i2c_write(0x80);
    // Set Pre-charge Period (Default 0x22).
    i2c_write(SSD1306_PRECHARGE);
    i2c_write(0x22);
    // Set Vcomh Deselect Level (Default 0x20).
    i2c_write(SSD1306_VCOMH_DESELECT_LEVEL);
    i2c_write(0x20);
    // Addressing.
    // Set Memory Page Addressing Mode (Default).
    i2c_write(SSD1306_MEMORY_ADDRESS_MODE);
    i2c_write(SSD1306_MEMORY_ADDRESS_PAGE);
    // Set Higher nibble for Column address start (Default 0x10).
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_MSB);
    // Set Lower nibble for Column address start (Default 0x00).
    i2c_write(SSD1306_COLUMN_ADDRESS_PAGE_LSB);
    // Set Page start (Default PAGE0).
    i2c_write(SSD1306_PAGE_ADDRESS_0);
    // Display Command.
    // Set contrast display 0x00 - 0xFF (Default 0x7F).
    i2c_write(SSD1306_DISPLAY_CONTRAST);
    i2c_write(0x7f);
    // Output follows RAM content (Default 0xA4).
    i2c_write(SSD1306_DISPLAY_ALL_POINT_RAM);
    // Normal Display (Default).
    i2c_write(SSD1306_DISPLAY_NORMAL);
    // Charge Pump & Display ON.
    i2c_write(SSD1306_CHARGE_PUMP);
    i2c_write(SSD1306_CHARGE_PUMP_ON);
    i2c_write(SSD1306_DISPLAY_ON);
    i2c_stop();
}

void lcd_setCursor(uint8_t u8Page, uint8_t u8MSB, uint8_t u8LSB)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(u8Page);
    i2c_write((uint8_t)(SSD1306_COLUMN_ADDRESS_PAGE_LSB + u8LSB));
    i2c_write((uint8_t)(SSD1306_COLUMN_ADDRESS_PAGE_MSB + u8MSB));
    i2c_stop();
}

void lcd_writeBattery(uint8_t u8Battery)
{
    uint8_t u8Column;

    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    for(u8Column=0; u8Column<14; u8Column++){
        i2c_write(au8Battery[u8Battery][u8Column]);
    }
    i2c_stop();
}

void lcd_writeCharacter(uint8_t u8Data)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    i2c_write(u8Data);
    i2c_stop();
}

void lcd_writeString(const uint8_t * u8Data)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    while(*u8Data != '\0'){
        ssd1306_writeFont(*u8Data++);
    }
    i2c_stop();
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Page, uint8_t u8MSB, uint8_t u8LSB)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(u8Page);
    i2c_write((uint8_t)(SSD1306_COLUMN_ADDRESS_PAGE_LSB + u8LSB));
    i2c_write((uint8_t)(SSD1306_COLUMN_ADDRESS_PAGE_MSB + u8MSB));
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    while(*u8Data != '\0'){
        ssd1306_writeFont(*u8Data++);
    }
    i2c_stop();
}

void ssd1306_writeFont(uint8_t u8Data)
{
    uint8_t u8Column;

    for(u8Column=0; u8Column<5; u8Column++){
        i2c_write(au8Font5x7[(uint8_t)(u8Data - 0x20)][u8Column]);
    }
    i2c_write(0x00);
}
```

## 2.PIC16F1773/6/7/8/9 Module I2C OLED 0.96 - Horizontal Mode.

```c
// Configuration Registers.
#pragma config FOSC = INTOSC, WDTE = OFF, PWRTE = OFF, MCLRE = ON, CP = OFF
#pragma config BOREN = OFF, CLKOUTEN = OFF, IESO = OFF, FCMEN = OFF
#pragma config WRT = OFF, PPS1WAY = ON, ZCD = OFF, PLLEN = OFF
#pragma config STVREN = ON, BORV = LO, LPBOR = OFF, LVP = ON

#include <xc.h>
#include <stdint.h>
#define _XTAL_FREQ 8000000
// PIC16F1773/6/8 - Compile with XC8(v2.20).
// PIC16F1773/6/8 - @8MHz Internal Oscillator.

// Module I2C - LCD Graphical - OLED0.96 128x64.
// SSD1306 - Memory Horizontal Addressing Mode.
// Strings & Patterns - v0.1 - 06/2020.

// PIC8-Bit Trainer.
// Jumpers.
// SDA - Close.
// SCL - Close.
// VCAP - Open.
// VEE - Open.
// BCKL - Open.
// SPI/I2C - I2C.

// Module I2C - OLED0.96.
// Jumpers.
// SDA - Open.
// SCL - Open.

// Definitions.
// I2C.
#define I2C_READ                             0x01
#define I2C_WRITE                            0x00
#define I2C_FSCL_HZ                          400000
#define I2C_ADD                              ((_XTAL_FREQ/(4*I2C_FSCL_HZ))-1)
// SSD1306 Address Select Bits.
#define SSD1306_ADDRESS_78                   0x78
#define SSD1306_ADDRESS_7A                   0x7A
// SSD1306 Configuration Registers.
#define SSD1306_PAGE_ADDRESS                 0x22
#define SSD1306_PAGE_ADDRESS_0               0xB0
#define SSD1306_PAGE_ADDRESS_1               0xB1
#define SSD1306_PAGE_ADDRESS_2               0xB2
#define SSD1306_PAGE_ADDRESS_3               0xB3
#define SSD1306_PAGE_ADDRESS_4               0xB4
#define SSD1306_PAGE_ADDRESS_5               0xB5
#define SSD1306_PAGE_ADDRESS_6               0xB6
#define SSD1306_PAGE_ADDRESS_7               0xB7
#define	SSD1306_COLUMN_ADDRESS               0x21
#define SSD1306_MEMORY_ADDRESS_MODE          0x20
#define SSD1306_MEMORY_ADDRESS_HORIZONTAL    0x00
#define SSD1306_MEMORY_ADDRESS_VERTICAL      0x01
#define SSD1306_MEMORY_ADDRESS_PAGE          0x02
#define	SSD1306_COLUMN_ADDRESS_PAGE_LSB      0x00
#define	SSD1306_COLUMN_ADDRESS_PAGE_MSB      0x10
#define SSD1306_DISPLAY_CONTRAST             0x81
#define SSD1306_DISPLAY_ALL_POINT_RAM        0xA4
#define SSD1306_DISPLAY_ALL_POINT_ON         0xA5
#define SSD1306_DISPLAY_NORMAL               0xA6
#define SSD1306_DISPLAY_INVERSE              0xA7
#define SSD1306_DISPLAY_OFF                  0xAE
#define SSD1306_DISPLAY_ON                   0xAF
#define SSD1306_START_LINE                   0x40
#define SSD1306_ADDRESS_COLUMN_NORMAL        0xA0
#define SSD1306_ADDRESS_COLUMN_REVERSE       0xA1
#define SSD1306_MULTIPLEX_RATIO              0xA8
#define SSD1306_COM_OUTPUT_NORMAL            0xC0
#define SSD1306_COM_OUTPUT_REVERSE           0xC8
#define SSD1306_OFFSET                       0xD3
#define	SSD1306_COM_PIN                      0xDA
#define	SSD1306_COM_PIN_SEQUENTIAL           0x02
#define	SSD1306_COM_PIN_ALTERNATIVE          0x12
#define	SSD1306_COM_PIN_DISABLE              0x20
#define	SSD1306_COM_PIN_ENABLE               0x22
#define SSD1306_CLOCK_RATIO                  0xD5
#define SSD1306_PRECHARGE                    0xD9
#define SSD1306_VCOMH_DESELECT_LEVEL         0xDB
#define SSD1306_NOOP                         0xE3
#define SSD1306_CHARGE_PUMP                  0x8D
#define SSD1306_CHARGE_PUMP_ON               0x14
#define SSD1306_CHARGE_PUMP_OFF              0x10
#define SSD1306_COMMAND                      0x00
#define SSD1306_DATA                         0x40
#define SSD1306_INITIALIZATION_DELAY_MS      400
// OLED 0.96 Configuration.
#define OLED096_CONFIGURATION_I2C_ADDRESS    SSD1306_ADDRESS_78
#define OLED096_CONFIGURATION_COLUMN         128
#define OLED096_CONFIGURATION_ROW            64
#define OLED096_CONFIGURATION_PAGE           8
#define OLED096_CONFIGURATION_LINE_1         0
#define OLED096_CONFIGURATION_LINE_2         1
#define OLED096_CONFIGURATION_LINE_3         2
#define OLED096_CONFIGURATION_LINE_4         3
#define OLED096_CONFIGURATION_LINE_5         4
#define OLED096_CONFIGURATION_LINE_6         5
#define OLED096_CONFIGURATION_LINE_7         6
#define OLED096_CONFIGURATION_LINE_8         7
// Battery Patterns.
#define BATTERY_EMPTY                        0
#define BATTERY_1_3                          1
#define BATTERY_2_3                          2
#define BATTERY_FULL                         3

// Functions Prototype.
void i2c_restart(void);
void i2c_start(void);
void i2c_stop(void);
void i2c_write(uint8_t u8Data);
void lcd_clearDisplay(void);
void lcd_clearLine(uint8_t u8Page);
void lcd_displayContrast(uint8_t u8Data);
void lcd_initialize(void);
void lcd_setArea(uint8_t u8ColumnStart, uint8_t u8ColumnEnd, uint8_t u8PageStart, uint8_t u8PageEnd);
void lcd_setCursor(uint8_t u8Page, uint8_t u8Cursor);
void lcd_writeBattery(uint8_t u8Battery, uint8_t u8Page, uint8_t u8Cursor);
void lcd_writeCharacter(uint8_t u8Data);
void lcd_writeString(const uint8_t * u8Data);
void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Page, uint8_t u8Cursor);
void ssd1306_writeFont(uint8_t u8Data);

// Strings.
const uint8_t au8Tronix[] = "Tronix I/O";
const uint8_t au8WWW[] = "www.tronix.io";
const uint8_t au8Open[] = "OPEN SMART";
const uint8_t au8Oled[] = "OLED 0.96 - SSD1306";

// Font 5x7.
const uint8_t au8Font5x7[96][5] = {
    {0x00, 0x00, 0x00, 0x00, 0x00}, // Space
    {0x00, 0x00, 0x5F, 0x00, 0x00}, // !
    {0x00, 0x07, 0x00, 0x07, 0x00}, // "
    {0x14, 0x7F, 0x14, 0x7F, 0x14}, // #
    {0x24, 0x2A, 0x7f, 0x2A, 0x12}, // $
    {0x23, 0x13, 0x08, 0x64, 0x62}, // %
    {0x36, 0x49, 0x55, 0x22, 0x50}, // &
    {0x00, 0x00, 0x03, 0x00, 0x00}, // '
    {0x00, 0x1C, 0x22, 0x41, 0x00}, // (
    {0x00, 0x41, 0x22, 0x1C, 0x00}, // )
    {0x14, 0x08, 0x3E, 0x08, 0x14}, // *
    {0x08, 0x08, 0x3E, 0x08, 0x08}, // +
    {0x00, 0x50, 0x30, 0x00, 0x00}, // ,
    {0x08, 0x08, 0x08, 0x08, 0x08}, // -
    {0x00, 0x60, 0x60, 0x00, 0x00}, // .
    {0x20, 0x10, 0x08, 0x04, 0x02}, // /
    {0x3E, 0x51, 0x49, 0x45, 0x3E}, // 0
    {0x00, 0x42, 0x7F, 0x40, 0x00}, // 1
    {0x42, 0x61, 0x51, 0x49, 0x46}, // 2
    {0x21, 0x41, 0x45, 0x4B, 0x31}, // 3
    {0x18, 0x14, 0x12, 0x7F, 0x10}, // 4
    {0x27, 0x45, 0x45, 0x45, 0x39}, // 5
    {0x3C, 0x4A, 0x49, 0x49, 0x30}, // 6
    {0x03, 0x71, 0x09, 0x05, 0x03}, // 7
    {0x36, 0x49, 0x49, 0x49, 0x36}, // 8
    {0x06, 0x49, 0x49, 0x29, 0x1E}, // 9
    {0x00, 0x36, 0x36, 0x00, 0x00}, // :
    {0x00, 0x56, 0x36, 0x00, 0x00}, // ;
    {0x08, 0x14, 0x22, 0x41, 0x00}, // <
    {0x14, 0x14, 0x14, 0x14, 0x14}, // =
    {0x00, 0x41, 0x22, 0x14, 0x08}, // >
    {0x02, 0x01, 0x51, 0x09, 0x06}, // ?
    {0x32, 0x49, 0x79, 0x41, 0x3E}, // @
    {0x7E, 0x11, 0x11, 0x11, 0x7E}, // A
    {0x7F, 0x49, 0x49, 0x49, 0x36}, // B
    {0x3E, 0x41, 0x41, 0x41, 0x22}, // C
    {0x7F, 0x41, 0x41, 0x41, 0x3E}, // D
    {0x7F, 0x49, 0x49, 0x49, 0x41}, // E
    {0x7F, 0x09, 0x09, 0x09, 0x01}, // F
    {0x3E, 0x41, 0x41, 0x49, 0x7A}, // G
    {0x7F, 0x08, 0x08, 0x08, 0x7F}, // H
    {0x00, 0x41, 0x7F, 0x41, 0x00}, // I
    {0x20, 0x40, 0x41, 0x3F, 0x01}, // J
    {0x7F, 0x08, 0x14, 0x22, 0x41}, // K
    {0x7F, 0x40, 0x40, 0x40, 0x40}, // L
    {0x7F, 0x02, 0x0C, 0x02, 0x7F}, // M
    {0x7F, 0x04, 0x08, 0x10, 0x7F}, // N
    {0x3E, 0x41, 0x41, 0x41, 0x3E}, // O
    {0x7F, 0x09, 0x09, 0x09, 0x06}, // P
    {0x3E, 0x41, 0x51, 0x21, 0x5E}, // Q
    {0x7F, 0x09, 0x19, 0x29, 0x46}, // R
    {0x26, 0x49, 0x49, 0x49, 0x32}, // S
    {0x01, 0x01, 0x7F, 0x01, 0x01}, // T
    {0x3F, 0x40, 0x40, 0x40, 0x3F}, // U
    {0x1F, 0x20, 0x40, 0x20, 0x1F}, // V
    {0x3F, 0x40, 0x38, 0x40, 0x3F}, // W
    {0x63, 0x14, 0x08, 0x14, 0x63}, // X
    {0x07, 0x08, 0x70, 0x08, 0x07}, // Y
    {0x61, 0x51, 0x49, 0x45, 0x43}, // Z
    {0x00, 0x7F, 0x41, 0x41, 0x00}, // [
    {0x02, 0x04, 0x08, 0x10, 0x20}, //
    {0x00, 0x41, 0x41, 0x7F, 0x00}, // ]
    {0x04, 0x02, 0x01, 0x02, 0x04}, // ^
    {0x40, 0x40, 0x40, 0x40, 0x40}, // _
    {0x00, 0x01, 0x02, 0x04, 0x00}, // '
    {0x20, 0x54, 0x54, 0x54, 0x78}, // a
    {0x7F, 0x48, 0x44, 0x44, 0x38}, // b
    {0x38, 0x44, 0x44, 0x44, 0x20}, // c
    {0x38, 0x44, 0x44, 0x48, 0x7F}, // d
    {0x38, 0x54, 0x54, 0x54, 0x18}, // e
    {0x08, 0x7E, 0x09, 0x01, 0x02}, // f
    {0x08, 0x54, 0x54, 0x54, 0x3C}, // g
    {0x7F, 0x08, 0x04, 0x04, 0x78}, // h
    {0x00, 0x48, 0x7D, 0x40, 0x00}, // i
    {0x20, 0x40, 0x44, 0x3D, 0x00}, // j
    {0x7F, 0x10, 0x28, 0x44, 0x00}, // k
    {0x00, 0x41, 0x7F, 0x40, 0x00}, // l
    {0x7C, 0x04, 0x78, 0x04, 0x78}, // m
    {0x7C, 0x08, 0x04, 0x04, 0x78}, // n
    {0x38, 0x44, 0x44, 0x44, 0x38}, // o
    {0x7C, 0x14, 0x14, 0x14, 0x08}, // p
    {0x08, 0x14, 0x14, 0x18, 0x7C}, // q
    {0x7C, 0x08, 0x04, 0x04, 0x08}, // r
    {0x48, 0x54, 0x54, 0x54, 0x20}, // s
    {0x04, 0x3F, 0x44, 0x40, 0x20}, // t
    {0x3C, 0x40, 0x40, 0x20, 0x7C}, // u
    {0x1C, 0x20, 0x40, 0x20, 0x1C}, // v
    {0x3C, 0x40, 0x30, 0x40, 0x3C}, // w
    {0x44, 0x28, 0x10, 0x28, 0x44}, // x
    {0x0C, 0x50, 0x50, 0x50, 0x3C}, // y
    {0x44, 0x64, 0x54, 0x4C, 0x44}, // z
    {0x00, 0x08, 0x36, 0x41, 0x00}, // {
    {0x00, 0x00, 0x7F, 0x00, 0x00}, // |
    {0x00, 0x41, 0x36, 0x08, 0x00}, // }
    {0x10, 0x08, 0x08, 0x10, 0x08}, // ~
    {0x06, 0x09, 0x09, 0x06, 0x00}  // °
};

// Battery Patterns.
const uint8_t au8Battery[5][14] = {
    {0x7e, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x7e, 0x3c, 0x3c}, // Battery Empty.
    {0x7e, 0x42, 0x5a, 0x5a, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x42, 0x7e, 0x3c, 0x3c}, // Battery 1/3.
    {0x7e, 0x42, 0x5a, 0x5a, 0x42, 0x5a, 0x5a, 0x42, 0x42, 0x42, 0x42, 0x7e, 0x3c, 0x3c}, // Battery 2/3.
    {0x7e, 0x42, 0x5a, 0x5a, 0x42, 0x5a, 0x5a, 0x42, 0x5a, 0x5a, 0x42, 0x7e, 0x3c, 0x3c}  // Battery Full.
};

// Main.
int main(void)
{
    // MCU Initialization.
    // Internal Oscillator.
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
    TRISC = 0b00011000;
    TRISE = 0b00000000;
    // WPU Disable.
    OPTION_REGbits.nWPUEN = 0b1;
    // LATCH Outputs.
    LATA = 0b00000000;
    LATB = 0b00000000;
    LATC = 0b00011000;
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
    // PPS Inputs.
    SSPDATPPSbits.SSPDATPPS = 0x14;  // RC4 - MSSP.SDA.
    // PPS Outputs.
    RC3PPSbits.RC3PPS = 0x21;        // RC3 - MSSP.SCL.
    RC4PPSbits.RC4PPS = 0x22;        // RC4 - MSSP.SDA.
    PPSLOCK = 0x55;
    PPSLOCK = 0xAA;
    PPSLOCKbits.PPSLOCKED = 0b1;

    // I2C Master Settings.
    SSP1BUF = 0;
    SSP1ADD = I2C_ADD;
    SSP1STAT = 0x00;
    SSP1CON1 = 0x08;
    SSP1CON2 = 0x00;
    SSP1CON3 = 0x00;
     // I2C Enable.
    SSP1CON1bits.SSPEN = 0b1;

    // ST7036 Initialization.
    lcd_initialize();
    lcd_clearDisplay();

    // Display Strings & Pattern.
    lcd_writeStringSetCursor(au8Tronix, OLED096_CONFIGURATION_LINE_1, 37);
    lcd_writeStringSetCursor(au8WWW, OLED096_CONFIGURATION_LINE_2, 22);
    lcd_writeStringSetCursor(au8Open, OLED096_CONFIGURATION_LINE_4, 32);
    lcd_writeStringSetCursor(au8Oled, OLED096_CONFIGURATION_LINE_5, 8);
    lcd_writeBattery(BATTERY_FULL, OLED096_CONFIGURATION_LINE_1, 114);
    __delay_ms(3000);

    uint16_t u16Column;
    lcd_setArea(0, 127, OLED096_CONFIGURATION_LINE_3, OLED096_CONFIGURATION_LINE_8);
    while(1){
        // Checkerboard Pattern.
        i2c_start();
        i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
        i2c_write(SSD1306_DATA);
        for(u16Column=0; u16Column<((OLED096_CONFIGURATION_COLUMN/2)*6); u16Column++){
            i2c_write(0x55);
            i2c_write(0xaa);
        }
        i2c_stop();
        __delay_ms(1000);

        // Full Pattern.
        i2c_start();
        i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
        i2c_write(SSD1306_DATA);
        for(u16Column=0; u16Column<(OLED096_CONFIGURATION_COLUMN*6); u16Column++){
            i2c_write(0xff);
        }
        i2c_stop();
        __delay_ms(1000);
    }
    return(0);
}

// Functions.
void i2c_restart(void)
{
    while(SSP1CON2 & 0x1F){};
    SSP1CON2bits.RSEN = 0b1;
    while(SSP1CON2bits.RSEN){};
}

void i2c_start(void)
{
    while(SSP1CON2 & 0x1F){};
    SSP1CON2bits.SEN = 0b1;
    while(SSP1CON2bits.SEN){};
}

void i2c_stop(void)
{
    while(SSP1CON2 & 0x1F){};
    SSP1CON2bits.PEN = 0b1;
    while(SSP1CON2bits.PEN){};
}

void i2c_write(uint8_t u8Data)
{
    SSP1BUF = u8Data;
    while(SSP1STATbits.BF){};
}

void lcd_clearDisplay(void)
{
    uint16_t u8Column = ((OLED096_CONFIGURATION_COLUMN*OLED096_CONFIGURATION_ROW)/8);

    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(0);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(0);
    i2c_write(OLED096_CONFIGURATION_PAGE-1);
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    while(u8Column--){
        i2c_write(0x00);
    }
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(0);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(0);
    i2c_write(OLED096_CONFIGURATION_LINE_8);
    i2c_stop();
}

void lcd_clearLine(uint8_t u8Page)
{
    uint8_t u8Column = OLED096_CONFIGURATION_COLUMN;

    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(0);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(u8Page);
    i2c_write(u8Page);
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    while(u8Column--){
        i2c_write(0x00);
    }
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(0);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(u8Page);
    i2c_write(u8Page);
    i2c_stop();
}

void lcd_displayContrast(uint8_t u8Data)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_DISPLAY_CONTRAST);
    i2c_write(u8Data);
    i2c_stop();
}

void lcd_initialize(void)
{
    __delay_ms(SSD1306_INITIALIZATION_DELAY_MS);

    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    // Hardware.
    // Set display start line (Default 0x40).
    i2c_write(SSD1306_START_LINE);
    // Column address 127 is mapped to SEG0.
    i2c_write(SSD1306_ADDRESS_COLUMN_REVERSE);
    // Set Multiplex Ratio 0x00 - 0x3F (Default 0x3F).
    i2c_write(SSD1306_MULTIPLEX_RATIO);
    i2c_write(0x3f);
    // Set Remapped Mode COM[MUX_RATiO-1] to COM0.
    i2c_write(SSD1306_COM_OUTPUT_REVERSE);
    // Set Display Offset (Default 0x00);
    i2c_write(SSD1306_OFFSET);
    i2c_write(0x00);
    // Set COM Pins (Default 0x02)
    i2c_write(SSD1306_COM_PIN);
    i2c_write(SSD1306_COM_PIN_ALTERNATIVE);
    // Timing.
    // Set Display Clock Divide Ratio / Oscillator (Default 0x80).
    i2c_write(SSD1306_CLOCK_RATIO);
    i2c_write(0x80);
    // Set Pre-charge Period (Default 0x22).
    i2c_write(SSD1306_PRECHARGE);
    i2c_write(0x22);
    // Set Vcomh Deselect Level (Default 0x20).
    i2c_write(SSD1306_VCOMH_DESELECT_LEVEL);
    i2c_write(0x20);
    // Addressing.
    // Set Memory Horizontal Addressing Mode.
    i2c_write(SSD1306_MEMORY_ADDRESS_MODE);
    i2c_write(SSD1306_MEMORY_ADDRESS_HORIZONTAL);
    // Set Column address start & end (Default 0x00 & 0x7F).
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(0x00);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    // Set Page address start & end (Default 0x00 & 0x07).
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(0x00);
    i2c_write(OLED096_CONFIGURATION_PAGE-1);
    // Display Command.
    // Set contrast display 0x00 - 0xFF (Default 0x7F).
    i2c_write(SSD1306_DISPLAY_CONTRAST);
    i2c_write(0x7f);
    // Output follows RAM content (Default 0xA4).
    i2c_write(SSD1306_DISPLAY_ALL_POINT_RAM);
    // Normal Display (Default).
    i2c_write(SSD1306_DISPLAY_NORMAL);
    // Charge Pump & Display ON.
    i2c_write(SSD1306_CHARGE_PUMP);
    i2c_write(SSD1306_CHARGE_PUMP_ON);
    i2c_write(SSD1306_DISPLAY_ON);
    i2c_stop();
}

void lcd_setArea(uint8_t u8ColumnStart, uint8_t u8ColumnEnd, uint8_t u8PageStart, uint8_t u8PageEnd)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(u8ColumnStart);
    i2c_write(u8ColumnEnd);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(u8PageStart);
    i2c_write(u8PageEnd);
    i2c_stop();
}

void lcd_setCursor(uint8_t u8Page, uint8_t u8Cursor)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(u8Cursor);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(u8Page);
    i2c_write(u8Page);
    i2c_stop();
}

void lcd_writeBattery(uint8_t u8Battery, uint8_t u8Page, uint8_t u8Cursor)
{
    uint8_t u8Column;

    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(u8Cursor);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(u8Page);
    i2c_write(u8Page);
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    for(u8Column=0; u8Column<14; u8Column++){
        i2c_write(au8Battery[u8Battery][u8Column]);
    }
    i2c_stop();
}

void lcd_writeCharacter(uint8_t u8Data)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    i2c_write(u8Data);
    i2c_stop();
}

void lcd_writeString(const uint8_t * u8Data)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    while(*u8Data != '\0'){
        ssd1306_writeFont(*u8Data++);
    }
    i2c_stop();
}

void lcd_writeStringSetCursor(const uint8_t * u8Data, uint8_t u8Page, uint8_t u8Cursor)
{
    i2c_start();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_COMMAND);
    i2c_write(SSD1306_COLUMN_ADDRESS);
    i2c_write(u8Cursor);
    i2c_write(OLED096_CONFIGURATION_COLUMN-1);
    i2c_write(SSD1306_PAGE_ADDRESS);
    i2c_write(u8Page);
    i2c_write(u8Page);
    i2c_restart();
    i2c_write(OLED096_CONFIGURATION_I2C_ADDRESS | I2C_WRITE);
    i2c_write(SSD1306_DATA);
    while(*u8Data != '\0'){
        ssd1306_writeFont(*u8Data++);
    }
    i2c_stop();
}

void ssd1306_writeFont(uint8_t u8Data)
{
    uint8_t u8Column;

    for(u8Column=0; u8Column<5; u8Column++){
        i2c_write(au8Font5x7[(uint8_t)(u8Data - 0x20)][u8Column]);
    }
    i2c_write(0x00);
}
```

---
DISCLAIMER: THIS CODE IS PROVIDED WITHOUT ANY WARRANTY OR GUARANTEES.
USERS MAY USE THIS CODE FOR DEVELOPMENT AND EXAMPLE PURPOSES ONLY.
AUTHORS ARE NOT RESPONSIBLE FOR ANY ERRORS, OMISSIONS, OR DAMAGES THAT COULD
RESULT FROM USING THIS FIRMWARE IN WHOLE OR IN PART.