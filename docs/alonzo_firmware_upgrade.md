# Alonzo board bootloader and firmware upgrade guide

Upgrading firmware can be intimidating—many people fear the risk of *bricking* their device.

Fortunately, with **Animula hardware**, that’s not a concern. The design ensures that firmware upgrades (and downgrades) are **safe and reliable**. You can update your device with confidence.

We hope you enjoy the process—it’s simple, secure, and stress-free.

## Before upgrade the firmware

There’s a **specific relationship** between the version numbers of the **Laco compiler** and **Animula VM firmware**.

The version number follows a **triple format**: **major.middle.minor**. For example, in version **v0.0.1**:
- **major** is 0
- **middle** is 0
- **minor** is 1

It's simple, really.

### Version Compatibility

- **Major** or **middle** version numbers will change when there are **incompatible changes** with the previous version.
- If your **Laco compiler** is **v0.0.5**, it is **compatible** only with **v0.0.x** Animula VM firmware, where **x** can be any positive integer.
- Similarly, **v0.2.6** of the Laco compiler is compatible with **v0.2.x** firmware, and **v1.3.5** with **v1.3.x** firmware.

### What to Do If Versions Are Incompatible

*Note*: If you find that your **Laco compiler** is incompatible with your **Animula VM firmware**, you have two options:
1. Change the **Laco compiler Docker image** to a compatible version.
2. **Upgrade or downgrade** your firmware to match the compiler version.

The **downgrade process** is exactly the same as the **upgrade process**—so there’s no need to worry about complexity.

## Upgrade Animula VM firmware

Before starting, we assume you've already downloaded the new firmware from the [download page](/articles/misc/download).

Once you unzip the downloaded file, you should get a **firmware.bin** file. **Do not rename** this file. The bootloader specifically looks for a file named **firmware.bin** on the TF card and will automatically flash it.

### Steps to Upgrade:

1. Copy **firmware.bin** to your TF card.
2. Insert the TF card into your **Animula hardware**—for example, your **Alonzo board**.
3. Power on the device. You should see the **red LED light** turn on and start blinking, indicating that the firmware upgrade is in progress.

<center>
<img src="../img/red_led.jpg" title="Red LED" alt="Red LED" width="450"/>
</center>

Please don't power-off before you see the LED green light.

<center>
<img src="../img/green_led.jpg" title="Green LED" alt="Green LED" width="450"/>
</center>

## What to Do If Power Is Cut Off During Upgrade?

If the power is cut off before you see the **green LED light**, don't worry. Simply repeat the upgrade process from the beginning. As mentioned earlier, **it’s safe**, and the process won’t harm your device.

### Important Notes:

- **Firmware Removal**: Once the upgrade is finished, the **firmware.bin** file will be automatically removed from the TF card.

- **Automatic Program Execution**: If a **program.lef** file is present on the TF card, it will be automatically executed as soon as the **green LED light** appears, signaling that the firmware upgrade is complete.

## Upgrade the bootloader

Upgrading the bootloader is an **advanced task** and should only be attempted if you have experience working with **GNU/Linux** systems.

In most cases, you won’t need to upgrade the bootloader. However, there are two scenarios where you might consider flashing a new bootloader:

1. **Firmware Upgrade Failure**: If you are unable to successfully upgrade the Animula VM firmware, it may be due to a broken bootloader. In this case, you can try upgrading the bootloader.

2. **Installing an Enhanced Bootloader**: If you wish to install a more advanced version of the bootloader, proceed with caution. **This action is at your own risk**. Make sure you understand the consequences and the potential for bricking the device.

### The preparation

**Note**: All necessary components are included in the **complete version** pack.

To flash the bootloader, you’ll need the **Saruman debugger**. This debugger is only included in the **complete version** pack. If you purchased the **standard version**, you may need to borrow one from another user or source.

That said, every serious **Animula developer** should consider owning at least one **complete version** pack for the full range of capabilities.

<center>
<img src="../img/saruman_coin.png" title="Saruman debugger" alt="Saruman debugger" width="800"/>
</center>

A 10-pin ribbon that connects Saruman to Animula hardware, say, Alonzo board.

<center>
<img src="../img/10-pin-ribbon.jpg" title="10-pin ribbon" alt="10-pin ribbon" width="450"/>
</center>

A Type-C cable to power the Animula hardware.

<center>
<img src="../img/t-cable.jpg" title="TypeC-2-TypeC cable" alt="TypeC-2-TypeC" width="450"/>
</center>

### Flash the bootloader

First, please connect your Saruman debugger and Animula hardware like this picture:

<center>
<img src="../img/debug_conn.jpg" title="Debug connect" alt="Debug connect" width="450"/>
</center>

**NOTE: Don't forget to connect the Animula board (Alonzo board) to the power with Type-C cable, otherwise there's no device will be detected.**

Then insert Saruman to the USB slot of your PC.

Now you should see **ttyACM0** or **ttyACM1** device on you PC. If there are more serial ports, the index will increase accordingly.
```bash
ls /dev/ttyACM*
```
If you haven't installed **gdb-multiarch**, please install it. We assume you're using Debian or Ubuntu:
```bash
sudo apt install gdb-multiarch
```
You should be able to run gdb-multiarch now.
```bash
gdb-multiarch
(gdb) set debug arm
(gdb) target extended-remote /dev/ttyACM0
```
Let's say you've put the **bootloader.elf** in */dev/shm*:
```bash
(gdb) file /dev/shm/bootloader.elf
# Reading symbols from bootloader.elf...
(gdb) monitor swdp_scan
# Target voltage: 3.3V
# Available Targets:
# No. Att Driver
#  1      STM32F40x M4
(gdb) attach 1
# Attaching to program:  /dev/shm/zephyr.elf, Remote target
(gdb) load
# Loading section rom_start, size 0x198 lma 0x8000000
# Loading section text, size 0x7692 lma 0x80001c0
# Loading section .ARM.exidx, size 0x8 lma 0x8007854
# Loading section initlevel, size 0xb8 lma 0x800785c
# Loading section sw_isr_table, size 0x2b0 lma 0x8007914
# Loading section rodata, size 0x7fc lma 0x8007bc4
# Loading section datas, size 0xe0 lma 0x80083c0
# Loading section devices, size 0xe4 lma 0x80084a0
# Loading section k_mem_slab_area, size 0x38 lma 0x8008584
# Start address 0x08001f50, load size 34194
# Transfer rate: 35 KB/sec, 814 bytes/write.
```

## Test the Upgrade Process

Now, **disconnect the board** from the power source, then attempt to **upgrade the firmware** to ensure everything is working as expected. Follow the steps outlined earlier to complete the upgrade process.

If everything goes smoothly, your Animula hardware should be ready with the new firmware!
