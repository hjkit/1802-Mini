# 1802-big-mini-elfos-410-install-v4.img

This is an experimental ROM image for the 1802/Mini with expander card that seeks to add maximal firmware-level support for the 1802/Mini hardware.

> __Note that "Experimental" means that this may or may not work exactly as advertised, and in addition, recovery may require re-programming your EEPROM back to prior source. Also, at this time, this firmware requires an [expander card](https://github.com/dmadole/1802-Mini-Expander-RTC) on the 1802/Mini.__

Please start with reading the notes of prior releases in the [firmware folder](https://github.com/dmadole/1802-Mini/new/master/firmware) as this image builds upon them. In addition to the prior changes, this image includes:

* The "nitro" soft UART in integrated in BIOS replacing the prior bit-banged UART. As a result, bit-bang serial speed is now up to 19200 baud at 4 Mhz.
* This image has the bit-bang serial port speedfixed at 19200 baud with no autobaud. However, autobaud is still supported and the BIOS can be built for it.
* The "hydro" DMA IDE driver is integrated in BIOS replacing the prior IDE read and write routines, giving accellerated disk throughput without drivers.
* In the "EF4 Menu" there is now an additional option that invokes on an-board EEPROM utility allowing in-circuit reprogramming of the EEPROM. At this time, it only works through the soft UART.

Please let me know any issues found and and ideas for further enhancement.
