# About this Marlin Fork

This fork was configured for the **Printrbot Metal Simple** (Printrboard F4) with the following modifications:
- [E3D PT100](https://e3d-online.com/products/pt100-temperature-sensor) Thermocouple board, attached to A2 (See Below)
- 250mm extended Printrbot Bed (Orignal Printrbot upgrade)
- Printrbot Heated Bed (Original Printrbot upgrade - ~80deg max)
- E3D V6 with high temperature heater block and cartriage. 

The original `Configuration.h` file is based on [printrboardmodernmarlin](https://github.com/Printrbot/printrboardmodernmarlin) which also has excellent documentation on further changes and support for other Printrbot models and Marlin versions.

For flashing instructions, see the excellent [RepRap Guide](https://reprap.org/wiki/Printrboard#Loading_Firmware_.28Linux.29). I compiled using the PlatformIO auto_build.py and flashed using gfu-bootloader on Ubuntu 18.

## E3D PT100
I could not find clear documentation online for hooking this up to a Printrboard and the port assignments I found were a bit confusing.

This Configuration assumes the amplifier board is connected to the **EXP1 header** on the Printrboard (JP2 in the [schematic](https://reprap.org/wiki/File:Printrboard_RevF_Schematic150.png).
- GND to Pin 14
- +5V to Pin 13
- Signal to Pin 2 (A2)

If looking at the board with the USB port in the bottom left, 

For those compiling, note that A2 which is listed as pin 59 on the schematic, is actually pin 2 in Marlin.

## Build Notes
I had issues getting the machine to connect when attempting to use the E3D recommended temp sensor `20` configuration with Marlin 1.1.9. This seemed to have been resolved in 1.1.9.1.

The default sensor type from Printrbot, and that used for the heated bed, is type `7`.

Also, to compile with PlatformIO, I made the following change to the `platformio.ini` file, setting the board to `teensy2pp`:

```c
[env:at90USB1286_DFU]
platform      = teensy
framework     = arduino
#board         = at90USB1286
board 	      = teensy2pp
build_flags   = ${common.build_flags}
lib_deps      = ${common.lib_deps}
lib_ldf_mode  = deep+
extra_scripts = pre:buildroot/share/atom/create_custom_upload_command_DFU.py
```
## Compile & Flash Process on Ubuntu CLI
Note that `auto_build` only works in Python 2.7.

From top level Marlin directory:

1. `python buildroot/share/atom/auto_build.py clean`
2. `python buildroot/share/atom/auto_build.py build`
3. This will build the `.hex` file in .pioenvs/
4. Install the boot jumper on the board, and reset to place into DFU bootloader. Confirm using `lsusb` looking for Atmel DFU.
5. `sudo dfu-programmer at90usb1286 erase`
6. `sudo dfu-programmer at90usb1286 flash .pioenvs/at90USB1286_DFU/firmware.hex`
7. Remove boot jumper and reset board.


# Marlin 3D Printer Firmware (From Marlin Main Repo)
Marlin is the world's most popular open source firmware for Replicating Rapid Prototyper (RepRap) machines, commonly referred to as "3D printers." Marlin Firmware is highly efficient, running even on modest 16MHz embedded AVR processors. While Marlin 1.1 only supports ATmega AVR (Arduino, etc.) and AT90USB (Teensy++ 2.0), [Marlin 2.0](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x) also adds support for several ARM processors, including the SAM3X8E (Arduino Due), NXP LPC1768/LPC1769 ARM Cortex-M3 (Re-Arm, MKS SBASE, Smoothieboard), and ARM Cortex-M4 (Teensy 3.5/3.6, STM32F1/4/7).

A monumental amount of talent and effort goes into Marlin production, and thanks are due to many volunteers around the world. We work closely with the community, contributors, vendors, host and library developers, etc. to improve the quality, configurability, and compatibility of Marlin Firmware with a [huge variety](http://marlinfw.org/docs/configuration/configuration.html#motherboard) of boards. For the final 1.1 release we focused on code quality, performance, stability, and overall user experience. Several new features were added, many of which require no extra hardware.

## Documentation

- Visit [marlinfw.org](http://marlinfw.org/) for complete documentation on [configuration](http://marlinfw.org/docs/configuration/configuration.html), [installation](http://marlinfw.org/docs/basics/install.html), [features](http://marlinfw.org/meta/features/), and the many [G-codes](http://marlinfw.org/meta/gcode/) that Marlin supports. We will continue to expand the site to include in-depth articles, tutorials, and how-to videos on all of Marlin's features.
- See the [Releases](https://github.com/MarlinFirmware/Marlin/releases) page for Release Notes on all current and previous versions of Marlin.
- Check out the [RepRap.org Marlin Page](http://reprap.org/wiki/Marlin) for an overview of Marlin and its role in the RepRap project.

## Marlin 1.1.x

The 1.1.x branch is home to all tagged releases of Marlin 1.1.

Marlin 1.1.9 is the final release of the AVR-only flat version of Marlin Firmware, so there will be no further 1.1.x releases. However [`bugfix-1.1.x`](https://github.com/MarlinFirmware/Marlin/tree/bugfix-1.1.x) will continue to receive patches for critical bugs, so be sure to test it (or [`bugfix-2.0.x`](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x)) before reporting any bugs you find in 1.1.9.

## Credits

Marlin Admins:
 - Erik van der Zalm [[@ErikZalm](https://github.com/ErikZalm)]
 - Scott Lahteine [[@thinkyhead](https://github.com/thinkyhead)]
 - Roxanne Neufeld [[@Roxy-3D](https://github.com/Roxy-3D)]
 - Bob Kuhn [[@Bob-the-Kuhn](https://github.com/Bob-the-Kuhn)]

Notable contributors:
 - Alberto Cotronei [[@MagoKimbra](https://github.com/MagoKimbra)]
 - Andreas Hardtung [[@AnHardt](https://github.com/AnHardt)]
 - Bernhard Kubicek [[@bkubicek](https://github.com/bkubicek)]
 - Bob Cousins [[@bobc](https://github.com/bobc)]
 - Chris Palmer [[@nophead](https://github.com/nophead)]
 - Chris Pepper [[@p3p](https://github.com/p3p)]
 - David Braam [[@daid](https://github.com/daid)]
 - Éduardo Tagle [[@ejtagle](https://github.com/ejtagle)]
 - Edward Patel [[@epatel](https://github.com/epatel)]
 - Ernesto Martinez [[@emartinez167](https://github.com/emartinez167)]
 - F. Malpartida [[@fmalpartida](https://github.com/fmalpartida)]
 - Giuliano Zaro [[@GMagician](https://github.com/GMagician)]
 - Jochen Groppe [[@CONSULitAS](https://github.com/CONSULitAS)]
 - João Brazio [[@jbrazio](https://github.com/jbrazio)]
 - Kai [[@Kaibob2](https://github.com/Kaibob2)]
 - Luc Van Daele[[@LVD-AC](https://github.com/LVD-AC)]
 - Nico Tonnhofer [[@Wurstnase](https://github.com/Wurstnase)]
 - Petr Zahradnik [[@clexpert](https://github.com/clexpert)]
 - Thomas Moore [[@tcm0116](https://github.com/tcm0116)]
 - [[@alexxy](https://github.com/alexxy)]
 - [[@android444](https://github.com/android444)]
 - [[@benlye](https://github.com/benlye)]
 - [[@bgort](https://github.com/bgort)]
 - [[@Grogyan](https://github.com/Grogyan)]
 - [[@marcio-ao](https://github.com/marcio-ao)]
 - [[@maverikou](https://github.com/maverikou)]
 - [[@oysteinkrog](https://github.com/oysteinkrog)]
 - [[@p3p](https://github.com/p3p)]
 - [[@paclema](https://github.com/paclema)]
 - [[@paulusjacobus](https://github.com/paulusjacobus)]
 - [[@psavva](https://github.com/psavva)]
 - [[@Tannoo](https://github.com/Tannoo)]
 - [[@teemuatlut](https://github.com/teemuatlut)]
 - ...and you!

## License

Marlin is published under the [GPL license](https://github.com/COPYING.md) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.

---