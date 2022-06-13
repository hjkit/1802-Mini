# 1802-big-mini-elfos-410-install-v4.1.img

This is an experimental ROM image for the 1802/Mini with expander card that seeks to add maximal firmware-level support for the 1802/Mini hardware.

> __Note that "Experimental" means that this may or may not work exactly as advertised, and in addition, recovery may require re-programming your EEPROM. Also, at this time, this firmware requires an [expander card](https://github.com/dmadole/1802-Mini-Expander-RTC) on the 1802/Mini.__

Please start with reading the notes of prior releases in the [firmware folder](https://github.com/dmadole/1802-Mini/new/master/firmware) as this image builds upon them. In addition to the prior changes, this image includes:

* The "nitro" soft UART in integrated in BIOS replacing the prior bit-banged UART. As a result, bit-bang serial speed is now up to 19200 baud at 4 Mhz.
* This image has the bit-bang serial port speed fixed at 19200 baud with no autobaud. However, autobaud is still supported and the BIOS can be built for it.
* The "hydro" DMA IDE driver is integrated in BIOS replacing the prior IDE read and write routines, giving accellerated disk throughput without drivers.
* In the "EF4 Menu" there is now an additional option that invokes on an-board EEPROM utility allowing in-circuit reprogramming of the EEPROM.

In addition to the above functional changes, all BIOS contents excepting the f_findtkn and f_idnum routines have been rewritten from scratch which has brought a trivial performance increase and a significant memory footprint decrease. Including savings from prior removal of f_mover and f_minimon, the BIOS is now down to 1500 bytes, leaving 548 bytes within my target 2048 byte space for further hardware support and improvement. Future versions will reduce the size slightlymore  through continued optimizataion and from moving initialization code out of the permanently resident portion of the ROM.

### Quick notes on the EEPROM utility:

The EEPROM utility now works on either the soft UART or hardware 1854 UART. The EEPROM is programmed using a 32K ROM image sent via XMODEM through the serial port.

Commands:

* Test - performs the XMODEM transfer without changing EEPROM to verify setup
* Unprotect - removes software write protect from the EEPROM
* Write - receives image via XMODEM and programs to EEPROM
* Verify - receives image via XMODEM and compared to EEPROM
* Protect - activates software write protect on the EEPROM
* Boot - Jumps to 8000h to boot the system.

Note that for this to work, the EEPROM jumpers must be set to allow hardware write (same as the RAM socket). The EEPROM should always be software write-protected when it is not hardware write-protected to prevent accidental modification.
