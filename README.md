# Marlin 3D Printer Firmware

![GitHub](https://img.shields.io/github/license/marlinfirmware/marlin.svg)
![GitHub contributors](https://img.shields.io/github/contributors/marlinfirmware/marlin.svg)
![GitHub Release Date](https://img.shields.io/github/release-date/marlinfirmware/marlin.svg)
[![Build Status](https://github.com/MarlinFirmware/Marlin/workflows/CI/badge.svg?branch=bugfix-2.0.x)](https://github.com/MarlinFirmware/Marlin/actions)

<img align="right" width=175 src="buildroot/share/pixmaps/logo/marlin-250.png" />

Additional documentation can be found at the [Marlin Home Page](https://marlinfw.org/).
Please test this firmware and let us know if it misbehaves in any way. Volunteers are standing by!

## Branch hardware/ender3pro/skr-v1.4-turbo

### Hardware in use

* Frame: Ender 3 Pro (v1)
* Mainboard: BigTreeTech SKR v1.4 Turbo
* Stepper drivers: BigTreeTech TMC2209
* Display: Original Universal LCD 12864 Creality CR10
* Extruder:
  * Type: Direct
  * Hotend: BMG Aero Volcano
  * Gears: BMG Aero Volcano

## Documentation

### Unified Bed Leveling

<https://marlinfw.org/docs/features/unified_bed_leveling.html#unified-bed-leveling>

## Debug Info

### EndStops

View endstop statuses with `M119`

### G28 and G29 commands

* Enable `DEBUG_LEVELING_FEATURE` and `M114_DETAIL`
* Enable (in ubl.h) or add `#define UBL_DEVEL_DEBUGGING`(to Configuration.h)
* Build and flash the Firmware to your 3D Printer board
* Connect to your printer from host software such as Octoprint, Cura, Printrun or Repetier Host
* Issue the command `M111 S247` to enable maximum logging (or `M111 S32` for Bed Leveling logging)
* Perform a `G28` to do your standard homing procedure
* Do a `G29 W` to view debug data for UBL
* Do a `G29 P1` to start Phase 1 of UBL bed leveling

### EEPROM

* `M502` Factory reset
* `M500` Save settings to EEPROM
* `M501` Load settings from EEPROM

### Calibration

#### Determining steps/mm (S)

* Heat the hot-end to material nominal temperature
* `G1 E100 F100`
* Measure the extruded length (ml)
* Find current value set (using m503) (cl)
* Calculate new S = 100/ml * cl
* <https://www.thingiverse.com/thing:2484766>

#### Determining Default_Kp Default_Ki Default_Kd

* `M303 C8 S200 U1`
* get resulted values from firmware response

### Thermistor calibration

1 Connect the calibrated thermistor to Hotend connector on the main board
1 Set `TEMP_SENSOR_0` to proper model for the calibrated thermistor
1 Insert the thermistor that will be calibrated to bed thermistor connector on the main board
1 Change `HEATER_0_MAXTEMP` to reflect desired max temperature to reach by calibrated thermistor
1 Change `BED_MAXTEMP` to reflect desired max temperature to reach by calibrating thermistor
1 In `Configuration_adv.h` uncomment `#define SHOW_TEMP_ADC_VALUES`
1 Use Pronterface to connect to printer and set temperature while reading ADC values

## Marlin 2.0 Bugfix Branch

__Not for production use. Use with caution!__

Marlin 2.0 takes this popular RepRap firmware to the next level by adding support for much faster 32-bit and ARM-based boards while improving support for 8-bit AVR boards. Read about Marlin's decision to use a "Hardware Abstraction Layer" below.

Download earlier versions of Marlin on the [Releases page](https://github.com/MarlinFirmware/Marlin/releases).

## Building Marlin 2.0

To build Marlin 2.0 you'll need [Arduino IDE 1.8.8 or newer](https://www.arduino.cc/en/main/software) or [PlatformIO](https://docs.platformio.org/en/latest/ide.html#platformio-ide). We've posted detailed instructions on [Building Marlin with Arduino](https://marlinfw.org/docs/basics/install_arduino.html) and [Building Marlin with PlatformIO for ReArm](https://marlinfw.org/docs/basics/install_rearm.html) (which applies well to other 32-bit boards).

## Hardware Abstraction Layer (HAL)

Marlin 2.0 introduces a layer of abstraction so that all the existing high-level code can be built for 32-bit platforms while still retaining full 8-bit AVR compatibility. Retaining AVR compatibility and a single code-base is important to us, because we want to make sure that features and patches get as much testing and attention as possible, and that all platforms always benefit from the latest improvements.

### Current HALs

  #### AVR (8-bit)

  board|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Arduino AVR](https://www.arduino.cc/)|ATmega, ATTiny, etc.|16-20MHz|64-256k|2-16k|5V|no

  #### DUE

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Arduino Due](https://www.arduino.cc/en/Guide/ArduinoDue), [RAMPS-FD](https://www.reprap.org/wiki/RAMPS-FD), etc.|[SAM3X8E ARM-Cortex M3](https://www.microchip.com/wwwproducts/en/ATsam3x8e)|84MHz|512k|64+32k|3.3V|no

  #### ESP32

  board|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [ESP32](https://www.espressif.com/en/products/hardware/esp32/overview)|Tensilica Xtensa LX6|160-240MHz variants|---|---|3.3V|---

  #### LPC1768 / LPC1769

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Re-ARM](https://www.kickstarter.com/projects/1245051645/re-arm-for-ramps-simple-32-bit-upgrade)|[LPC1768 ARM-Cortex M3](https://www.nxp.com/products/microcontrollers-and-processors/arm-based-processors-and-mcus/lpc-cortex-m-mcus/lpc1700-cortex-m3/512kb-flash-64kb-sram-ethernet-usb-lqfp100-package:LPC1768FBD100)|100MHz|512k|32+16+16k|3.3-5V|no
  [MKS SBASE](https://reprap.org/forum/read.php?13,499322)|LPC1768 ARM-Cortex M3|100MHz|512k|32+16+16k|3.3-5V|no
  [Selena Compact](https://github.com/Ales2-k/Selena)|LPC1768 ARM-Cortex M3|100MHz|512k|32+16+16k|3.3-5V|no
  [Azteeg X5 GT](https://www.panucatt.com/azteeg_X5_GT_reprap_3d_printer_controller_p/ax5gt.htm)|LPC1769 ARM-Cortex M3|120MHz|512k|32+16+16k|3.3-5V|no
  [Smoothieboard](https://reprap.org/wiki/Smoothieboard)|LPC1769 ARM-Cortex M3|120MHz|512k|64k|3.3-5V|no

  #### SAMD51

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Adafruit Grand Central M4](https://www.adafruit.com/product/4064)|[SAMD51P20A ARM-Cortex M4](https://www.microchip.com/wwwproducts/en/ATSAMD51P20A)|120MHz|1M|256k|3.3V|yes

  #### STM32F1

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Arduino STM32](https://github.com/rogerclarkmelbourne/Arduino_STM32)|[STM32F1](https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html) ARM-Cortex M3|72MHz|256-512k|48-64k|3.3V|no
  [Geeetech3D GTM32](https://github.com/Geeetech3D/Diagram/blob/master/Rostock301/Hardware_GTM32_PRO_VB.pdf)|[STM32F1](https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html) ARM-Cortex M3|72MHz|256-512k|48-64k|3.3V|no

  #### STM32F4

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [STEVAL-3DP001V1](https://www.st.com/en/evaluation-tools/steval-3dp001v1.html)|[STM32F401VE Arm-Cortex M4](https://www.st.com/en/microcontrollers-microprocessors/stm32f401ve.html)|84MHz|512k|64+32k|3.3-5V|yes

  #### Teensy++ 2.0

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Teensy++ 2.0](https://www.microchip.com/wwwproducts/en/AT90USB1286)|[AT90USB1286](https://www.microchip.com/wwwproducts/en/AT90USB1286)|16MHz|128k|8k|5V|no

  #### Teensy 3.1 / 3.2

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Teensy 3.2](https://www.pjrc.com/store/teensy32.html)|[MK20DX256VLH7](https://www.mouser.com/ProductDetail/NXP-Freescale/MK20DX256VLH7) ARM-Cortex M4|72MHz|256k|32k|3.3V-5V|yes

  #### Teensy 3.5 / 3.6

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Teensy 3.5](https://www.pjrc.com/store/teensy35.html)|[MK64FX512VMD12](https://www.mouser.com/ProductDetail/NXP-Freescale/MK64FX512VMD12) ARM-Cortex M4|120MHz|512k|192k|3.3-5V|yes
  [Teensy 3.6](https://www.pjrc.com/store/teensy36.html)|[MK66FX1M0VMD18](https://www.mouser.com/ProductDetail/NXP-Freescale/MK66FX1M0VMD18) ARM-Cortex M4|180MHz|1M|256k|3.3V|yes

  #### Teensy 4.0 / 4.1

  boards|processor|speed|flash|sram|logic|fpu
  ----|---------|-----|-----|----|-----|---
  [Teensy 4.0](https://www.pjrc.com/store/teensy40.html)|[IMXRT1062DVL6A](https://www.mouser.com/new/nxp-semiconductors/nxp-imx-rt1060-crossover-processor/) ARM-Cortex M7|600MHz|1M|2M|3.3V|yes
  [Teensy 4.1](https://www.pjrc.com/store/teensy41.html)|[IMXRT1062DVJ6A](https://www.mouser.com/new/nxp-semiconductors/nxp-imx-rt1060-crossover-processor/) ARM-Cortex M7|600MHz|1M|2M|3.3V|yes

## Submitting Patches

## Marlin Support

- This branch is for fixing bugs and integrating any new features for the duration of the Marlin 2.0.x life-cycle.
- Follow the [Coding Standards](https://marlinfw.org/docs/development/coding_standards.html) to gain points with the maintainers.
- Please submit Feature Requests and Bug Reports to the [Issue Queue](https://github.com/MarlinFirmware/Marlin/issues/new/choose). Support resources are also listed there.

- [Marlin Documentation](http://marlinfw.org) - Official Marlin documentation
- [Marlin Discord](https://discord.gg/n5NJ59y) - Discuss issues with Marlin users and developers
- Facebook Group ["Marlin Firmware"](https://www.facebook.com/groups/1049718498464482/)
- RepRap.org [Marlin Forum](http://forums.reprap.org/list.php?415)
- [Tom's 3D Forums](https://forum.toms3d.org/)
- Facebook Group ["Marlin Firmware for 3D Printers"](https://www.facebook.com/groups/3Dtechtalk/)
- [Marlin Configuration](https://www.youtube.com/results?search_query=marlin+configuration) on YouTube

## Credits

The current Marlin dev team consists of:

 - Scott Lahteine [[@thinkyhead](https://github.com/thinkyhead)] - USA &nbsp; [Donate](http://www.thinkyhead.com/donate-to-marlin)
 - Roxanne Neufeld [[@Roxy-3D](https://github.com/Roxy-3D)] - USA
 - Chris Pepper [[@p3p](https://github.com/p3p)] - UK
 - Bob Kuhn [[@Bob-the-Kuhn](https://github.com/Bob-the-Kuhn)] - USA
 - Erik van der Zalm [[@ErikZalm](https://github.com/ErikZalm)] - Netherlands &nbsp; [![Flattr Erik](https://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software)

## License

Marlin is published under the [GPL license](/LICENSE) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.
