# E54Board
This is a quick guide for E54Board.

## Overview:

![alt text](png/E54Board.png)

## Components

| item                  | Model        | Note     | Status |
| --------------------- | ------------ | -------- | ------ |
| EEPROM                | AT24C02      | On board | PASS   |
| Crypto IC             | ATECC608A    | On board | PASS   |
| QSPI FLash            | SST26(Mhz)   | On board | PASS   |
| SWD                   | J-Link(4Mhz) | On board | PASS   |
| FPC (SPI+I2C)         | -            | On board | PASS   |
| WINC1500              | ATWINC1500   | optional |        |
| LCD                   |              | optional |        |
| micro SD              | Kingston 2GB | optional | PASS   |
| touch sensor          | -            | optional |        |
| Xplained I/O Expander |              | optional |        |



# Flash Bootloader

You can use J-Link or Atmel/Microchip programmer to program the bootloader.

# Flash User Application

## Supported OS

* Linux
* Windows

## Upload User Application

1. Plug the USB Connector to your computer.

2. Press the Boot Button.

3. A new disk will be found immediately. Drag the user upgrade application to the disk.

   ![alt text](png/disk.png) 

## Enter boot entry in USER Application

Configure pins MPLAB Harmony 3 in IDE:

![pin](png/pin.png)



Add EIC14 Interrupt function 

![eic](png/eic.png)



Modify the start of address 

![boot_addr](png/boot_addr.png)



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

##  Build User application

Launch the script `uf2conv.py` in uf2util:

```
python uf2conv.py xxx.hex --base 0x4000 --convert --output app.uf2
```

Program the device:

```
python uf2conv.py -D app.uf2
```

## Enter bootloader entry by Reset

In some applications, the user button is occupied and we  cannot trigger bootloader mode by pressing the button. However, the bootloader can  still be triggered by the Reset pin. One can  wire the Reset pin to ground and  the board will go bootloader mode immediately.

![](png/SWD.png)
