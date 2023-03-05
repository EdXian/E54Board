# E54Board
This is a quick guide for E54Board.

## Overview:

![](png\E54Board.png)

## Components

| item                  | Model      | note      |
| --------------------- | ---------- | --------- |
| EEPROM                | AT24C02    | Installed |
| Crypto IC             | ATECC608A  | Installed |
| QSPI FLash            | SST26      | Installed |
| SWD                   | -          | Installed |
| FPC (SPI+I2C)         | -          | Installed |
| WINC1500              | ATWINC1500 | optional  |
| LCD                   |            | optional  |
| micro SD              |            | optional  |
| touch sensor          |            | optional  |
| Xplained I/O Expander |            | optional  |





# FLASH User Application

1. Plug the USB Connector to your computer.

2. Press the Boot Button.

3. A new disk will be found immediately. Drag the user upgrade application to the disk.

   ![](png\disk.png) 

## Enter boot entry in USER Application

To make your application enter boot entry, you can add the following code to main:

````c
void EIC_Pin14Callback (uintptr_t context);
void EIC_Pin14Callback (uintptr_t context)
{
    asm("nop");
    *((volatile uint32_t *)(HSRAM_ADDR + HSRAM_SIZE - 4)) = 0xf01669ef;
    NVIC_SystemReset();
}

int main(){
```
 EIC_CallbackRegister(EIC_PIN_14, EIC_Pin14Callback, 0);
 EIC_InterruptEnable(EIC_PIN_14);
```
}
````





