# 1802-mini-elfos-410-install-v1.img

This is a ROM image that contains a modified Elf/OS 4.1.0 installer package combined with an enhanced BIOS that is built from sources forked from Mike Riley's official repository. This software is a derivative work of the standard Elf/OS installation ROM and BIOS, therefore the following license terms apply:

> This software is copyright 2004-2005 by Michael H Riley
> You have permission to use, modify, copy, and distribute
> this software so long as this copyright notice is retained.
> This software may not be used in commercial applications
> without express written permission from the author.

## Summary of changes to the installer package:

* The ROM proceeds directly to booting Elf/OS on reset rather than displaying the ROM menu
* The ROM menu may still be accessed by asserting EF4 during reset (such as by holding the INPUT button)
* Cursor positioning and other terminal controls have been removed from the top ROM menu
* The xr and xrb executables are both included so install can be completed with software or hardware UARTs

## Summary of changes to the contained BIOS:

* An 1854 hardware UART is supported on I/O ports 6 (data) and 7 (status/control)
* The last usable RAM address is fixed at $7FFF and RAM is not scanned by f_freemem
* The bit-banged serial port baud rate is set to 4800 baud at reset with no auto-detect
* Bit-banged serial will be used if a cable is detected, otherwise the 1854 will be used
* There is a 500ms delay before booting from IDE so that the CF card can become ready
* The undocumented f_mover BIOS call has been omitted from the ROM image to save space

Source code for the modified BIOS may be found at:

> https://github.com/dmadole/Elfos-bios

## Additional details of changes:

This was developed for the purpose of supporting the 1854 UART on the 1802/Mini but the UART support is generic and not tied to any particular 1854 implementation. The BIOS implementation of the 1854 driver does not support flow control.

This BIOS supports both the 1854 UART and the bit-banged UART on EF2 and Q. It determines which one to use by looking at the EF2 line to see if it a device is connected or not. For this to work, the normal resting state for EF2 with no cable connected needs to be low (asserted). On the 1802/Mini this can be accomplished by setting the jumper for the receive open state to "GND"; this is the jumper nearest pins 1 and 14 of U8 (the CD4011). If there is no 1854 present, the bit-banged UART will be used regardless of the line state.

Disabling the f_freemem scan and always indicating memory size as $7FFF enables the ability to jumper a 28C256 EEPROM in the system as write-enabled so that it can be programmed in-system. Otherwise, the memory scan will cause the system to crash when it hits the EEPROM address. When running with the EEPROM hardware-write-enabled, it is important to use the software-write-protect feature to prevent data from being accidentally written.

In order to install Elf/OS or access the tools such as SEDIT included in the ROM, you will need to assert EF4 during reset. Also, exiting from SEDIT will boot from disk unless EF4 is held down, in which case it will return to the main menu.

Some of the BIOS changes are dependent on knowledge of the processor clock frequency; this image was built presuming a 4Mhz clock. If you are using a different clock frequency the baud rate and IDE boot delay will be proportionally different. It is possible to rebuild the BIOS for another frequency, please reach out if you would like assistance with that.
