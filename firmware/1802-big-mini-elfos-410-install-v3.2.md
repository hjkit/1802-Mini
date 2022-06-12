# 1802-big-mini-elfos-410-install-v3.2.img

This is a ROM image that contains a modified Elf/OS 4.1.0 installer package combined with an enhanced BIOS that is built from sources forked from Mike Riley's official repository. This software is a derivative work of the standard Elf/OS installation ROM and BIOS, therefore the following license terms apply:

> This software is copyright 2004-2005 by Michael H Riley
> You have permission to use, modify, copy, and distribute
> this software so long as this copyright notice is retained.
> This software may not be used in commercial applications
> without express written permission from the author.

As of version 3.2, the intial ROM installer menu has been rewritten from scratch, and the ROM also includes an EEPROM utility that can re-program the EEPROM in-circuit making future BIOS upgrades much easier.

Also as of 3.2, this ROM will work on an 1802/Mini with __or__ without an Expander RTC card and adds features to support the expander card memory and real-time clock. Changes that are new to this 1802/Mini ROM release are display in _italics_.

## Summary of changes to the installer package:

* _The intial ROM menu menu has been rewritten from scratch and includes an EEPROM programming utility._
* _The initial ROM menu and EEPROM utility use self-contained UART drivers making them independent of BIOS._
* The ROM proceeds directly to booting Elf/OS on reset rather than displaying the ROM menu
* The ROM menu may still be accessed by asserting EF4 during reset (such as by holding the INPUT button)
* Cursor positioning and other terminal controls have been removed from the top ROM menu
* The xr and xrb executables are both included so install can be completed with software or hardware UARTs
* _The boot Elf/OS menu option enables expanded memory before invoking BIOS to boot, allowing use of 62K RAM._
* An additional menu option has been added to boot without enabling expansion RAM (base 32K RAM only)
* The message "Booting system..." is displayed before booting even when not using the installer menu.
* _Exiting from SEDIT will now return to the ROM menu even if EF4 is not still asserted._

## Summary of changes to the contained BIOS:

* An 1854 hardware UART is supported on I/O ports 6 (data) and 7 (status/control)
* The f_minimon BIOS call, which does not seem to be accessible, has been removed to save space.
* _An Epson RTC-72421 real-time clock is supported on I/O port 5 if detected by BIOS._
* The RAM size is determined by scanning in a way that is safe for hardware-write enabled EEPROM
* The bit-banged serial port baud rate is set to 4800 baud at reset with no auto-detect
* Bit-banged serial will be used if a cable is detected, otherwise the 1854 will be used
* There is a 500ms delay before booting from IDE so that the CF card can become ready
* The undocumented f_mover BIOS call has been omitted from the ROM image to save space

Source code for the modified BIOS may be found at:

> https://github.com/dmadole/Elfos-bios

## Additional details of changes:

This was developed for the purpose of supporting the 1854 UART on the 1802/Mini but the UART support is generic and not tied to any particular 1854 implementation. The BIOS implementation of the 1854 driver does not support flow control.

This BIOS supports both the 1854 UART and the bit-banged UART on EF2 and Q. It determines which one to use by looking at the EF2 line to see if it a device is connected or not. For this to work, the normal resting state for EF2 with no cable connected needs to be low (asserted). On the 1802/Mini this can be accomplished by setting the jumper for the receive open state to "GND"; this is the jumper nearest pins 1 and 14 of U8 (the CD4011). If there is no 1854 present, the bit-banged UART will be used regardless of the line state.

A new f_freemem BIOS call has been implemented which scans memory to determine how much is present and does so in a way that supports a hardware write-enabled EEPROM in the system without crashing. This allows support for in-circuit programming of the EEPROM while being able to support variable amounts of memory. When running with the EEPROM hardware-write-enabled, it is important to use the software-write-protect feature to prevent data from being accidentally written.

This BIOS supports an Epson RTC-72421 real-time clock chip on port 5 as is used on the 1802/Mini expander card. The BIOS interface implements f_gettod and f_settod to be fully compatible with Elf/OS using the same software interface as the STG clock based on the DS12887. When first bringing up a new system, or after changing the battery, it is important to set the time before trying to read the time, as the RTC chip needs to be initialized. Failure to do so might result in an attempt to read the time hanging the system.

Ports 1 and 5 will have data output to them even if the expander card is not present, so it is recommended that no other devices be on these ports. This means this ROM should not be used on a Super Elf with PIXIE installed; it will probably work on one without although this has not been fully tested.

In order to install Elf/OS or access the tools such as SEDIT included in the ROM, you will need to assert EF4 during reset.

Some of the BIOS changes are dependent on knowledge of the processor clock frequency; this image was built presuming a 4Mhz clock. If you are using a different clock frequency the baud rate and IDE boot delay will be proportionally different. It is possible to rebuild the BIOS for another frequency, please reach out if you would like assistance with that.
