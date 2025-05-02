<center>
<img src="../img/_MG_4241_transparent.png" alt="Animula Alonzo" width="800" />
</center>

## Features
The Animula Alonzo board features an ARM Cortex-M4 based STM32F411CE MCU
with a wide range of connectivity support and configurations. Here are some highlights of the Animula Alonzo board:

* STM32 microcontroller in UFQFN48 package
* It comes out with these resources:
    * 2.54 Header connecters
    * Bluetooth 4.0 BLE
    * 1 RGB LED and 1 white LED: D6 and D5
    * 3 buttons: BOOT0, RST and 1 User Button
    * TF card slot, user can compile program to files in TF card, virtual machine will load program from TF card.

## Zephyr Features
The Zephyr animula_alonzo board configuration supports the following hardware features:
* GPIO, TIMER, UART, SPI, I2C
* detailed configurations are in the file [alonzo_defconfig](https://github.com/hardenedlinux/animula-zephyr/blob/master/boards/animula_alonzo.conf).

Default Zephyr Peripheral Mapping:

* UART_1 TX/RX          : PB9/PA10
* UART_2 TX/RX          : PA2/PA3 (Connected to BLE chip)
* I2C2 SCL/SDA          : PB10/PB3
* I2C3 SCL/SDA          : PA8/PB4
* SPI1 CS/SCK/MISO/MOSI : PA4/PA5/PA6/PA7 (Connected to TF card socket)
* SPI2 CS/SCK/MISO/MOSI : PB12/PB13/PB14/PB15
* User button           : PA1
* LD2                   : PA5
* TIM4 CH1/CH2/CH3/CH4  : PB6/PB7/PB8/PB9

## Pin description
<center>
<img src="../img/pins_description_20201231_1250_875.png" alt="pin description" width="900" />
</center>


## Hardware spec

* [STM32F411CEU6](https://www.st.com/en/microcontrollers-microprocessors/stm32f411.html)
* ARM Cortex-M4 MCU, 512KB flash, 128KB RAM, 100MHz, 1.7-3.6V, 36 GPIOs in UFQFN48 package
* STM32F411CET6 in UFQFN48 package
* ARM |reg| 32-bit Cortex |reg|-M4 CPU with FPU
* 100 MHz max CPU frequency
* VDD from 1.7 V to 3.6 V
* 512 KB Flash
* 128 KB SRAM
* GPIO with external interrupt capability
* 12-bit ADC with 16 channels, with FIFO and burst support
* 8 General purpose timers
* 2 watchdog timers (independent and window)
* SysTick timer
* USART/UART (3)
* I2C (3)
* SPI (5)
* SDIO
* USB 2.0 OTG FS
* DMA Controller
* CRC calculation unit
* BLE
    * FreqChip FR8016HA BLE chip
    * [FR8016HA datasheet](https://newwezhanoss.oss-cn-hangzhou.aliyuncs.com/contents/sitefiles2038/10193999/files/200129..pdf)
* Li-ion Battery Charging Controller
    * [4056](http://www.tp4056.com/)
    * [4056 datasheet](http://www.tp4056.com/d/tp4056.pdf)
* Battery connector
    * 2.0mm edge battery socket
* TF card socket
    * User can compile the code into program.lef and copy to tf card, the program will load it automatically
