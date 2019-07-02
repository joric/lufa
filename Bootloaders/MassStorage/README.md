# USB Mass Storage bootloader for Pro Micro

## Download

* [BootloaderMassStorage.hex](https://raw.githubusercontent.com/joric/lufa/promicro/Bootloaders/MassStorage/precompiled/BootloaderMassStorage.hex)

* [FLASH.BIN](https://raw.githubusercontent.com/joric/lufa/promicro/Bootloaders/MassStorage/precompiled/FLASH.BIN) (LED 17 blinker)

## Video

[![](http://img.youtube.com/vi/S4cgjP2mC5c/0.jpg)](https://youtu.be/S4cgjP2mC5c)

## Building

Edit the configuration file like this:

```
MCU          = atmega32u4
ARCH         = AVR8
BOARD        = NONE
F_CPU        = 16000000
...
FLASH_SIZE_KB         = 32
BOOT_SECTION_SIZE_KB  = 4
```

Run `make`. In case of this:

```
/usr/lib/gcc/avr/4.9.2/../../../avr/bin/ld:
section .apitable_trampolines loaded at [0000000000007fa0,0000000000007fb7]
overlaps section .data loaded at [0000000000007e28,0000000000007fa7]
```

Make sure that `arm-gcc --version` is at least 5.4.0 (I'm using WSL on Windows 10 with Ubuntu 18.04).

## Uploading

You will need USBASP programmer to flash the bootloader and update the fuses. Wire it up and run the following:

```
avrdude -c usbasp -p atmega32u4 -U flash:w:BootloaderMassStorage.hex
```

## Usage

Hold Reset (short RST to GND) on power on for USB drive. Convert userspace firmware .hex files to .bin like so:

```
avr-objcopy -I ihex input.hex -O binary FLASH.BIN
```

Replace the old FLASH.BIN on USB drive (delete the old one first), then reset (power cycle) the Pro Micro.

## References

* https://github.com/abcminiuser/lufa/issues/148


