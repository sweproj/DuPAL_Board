# DuPAL board

## Introduction

The DuPAL board is a simple circuit that mounts an AVR MCU (A classic ATmega328P of Arduino fame), three 74HC595 SIPO registers, one 74HCT166 PISO register, a MAX232 to adapt the serial port to RS232, a linear voltage regulator and a bunch of other passive components.

This board was designed and built to help myself bruteforce PAL devices. It has the hardware facilities necessary to scan all the inputs of the PAL, to check whether an output is high, low or hi-z and to report back the status to the host.

Details on how the features are implemented can be found on the Firmware's repository.

![Rev. 2.0 PCB](pics/rev2.0_pcb.png)

## Software

### Bootloader

This board uses the [Optiboot Bootloader](https://github.com/Optiboot/optiboot), currently at version 8.0, built for an **ATMega328** running at 20Mhz, and with a baudrate of 57600bps.

The command line to build the bootloader is

```shell
make atmega328 AVR_FREQ=20000000L LED_START_FLASHES=8 BAUD_RATE=57600
```

See [Compiling Optiboot](https://github.com/Optiboot/optiboot/wiki/CompilingOptiboot) for details.

The resulting bootloader can then be loaded via the ISP header. For example, by using `avrdude` and an **AVR Dragon** programmer:

```shell
avrdude  -c dragon_isp -P usb \
  -p atmega328p -e -u -U efuse:w:0xFD:m -U hfuse:w:0xDE:m \
  -U lfuse:w:0xFF:m -U flash:w:optiboot_atmega328.hex
```

### Firmware

Details on the firmware can be found on the **DuPAL_Firmware** repository.

## Hardware

The board was designed with [KiCad](https://kicad-pcb.org/) EDA.

In its current form, the board is a pretty simple build, using exclusively through-hole components.

### Powering the board

The board can be powered with a DC supply voltage between 7.5V and 9V, 12V is possible but the regulator starts to get hot.

The power connector has a positive tip, and is protected against reverse voltage by a 1N4001 diode.

### Bill of Materials

**TODO for rev. 2**

