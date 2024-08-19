# Marlin Fork for Micromake D1

This is a fork of the Marlin 2.1 firmware based on the [fork by donbakala](https://github.com/donbakala/Marlin-Micromake-D1). This version adds some improvements, such as enabling auto-leveling,
adding the option to load/change/unload filament right from the menu of the printer etc.
The firmware that shipped with the printer from the factory is outdated and lacks many features compared to current firmwares, so I would highly recommend updating the firmware. For me, this made
the printer much more usable.

## Instructions for flashing the firmware

The microcontroller of the printer has an internal non-volatile memory (EEPROM) where the configuration of the printer is saved. After updating to the new Marlin 2.1 firmware, the printer
will promt you to initialize the EEPROM, which clears all the old contents. Therefore, I would recommend to save your current EEPROM settings in case you are not satisfied with the new firmware
and want to go back.
One way to do this is to open the original Cura 15.4 that shipped with the machine and select Machine -> Firmware configuration while your printer is connected to your PC. Now you can write down
all the parameters or, like I did it, take some screenshots.

To flash the new Marlin 2.1 firmware onto your Micromake D1, the Arduino IDE is required. See the instructions below ("Building Marlin 2.1") if you are not familiar with the Arduino IDE.
After installing it, connect your printer to the USB port of your computer and select the COM port in the menu of the Arduino IDE. If no COM port shows up after connecting the printer, you
probably need to install the [CH340 serial driver](https://www.wch-ic.com/downloads/CH341SER_EXE.html).
After selecting the COM port, click on the Upload button. This process may take several minutes, after which the printer should reboot and show the Marlin intro screen.

**ATTENTION: I have not tested if the new firmare still works with the ancient Cura 15.04 that is shipped with the printer! If you want to continue this version (which I would not recommend at all, see below), please note that it may not work!**

## Recommended hardware upgrades

Before printing with the new firmware, I would advise you to check the following things:
- Clean your printer nozzle or replace it with a new one. I would highly recommend not using a cheap nozzle or one of the nozzles that shipped with the printer, since their quality is lacking to
  say the least. I replaced the nozzle that shipped with the machine with a genuine E3D brass nozzle (0.4 mm), which can be had for less than 10 $. This increased the quality of my prints noticeably.
  While I was at it, I also cleaned the hotend.
- The printer has a Z probe to measure the distance from the nozzle to the heatbed (auto-leveling). This is a good idea, but the design of the Z probe is really bad, resulting in inaccurate
  measurements. I was never able to get a good first layer and many of my prints failed due to the bad auto-leveling.
  I printed [this Z probe](https://www.thingiverse.com/thing:2258863) by jazman and never looked back - it is a night and day difference compared to the original Z probe.
  The only things you need are a bit of filament and an M4 bolt + nut, all the other parts are already built into your printer.

## Auto-leveling procedure

The auto-leveling procedure works a bit different on the new firmware. Here is the procedure:

- Turn on your printer. If you printed the Z probe I recommended above, flap it down so that the limit switch is pointing to the build plate.
- Press the knob to go to the menu. Select Advanced Settings -> Delta calibration -> Auto calibration. After homing, the printer will start the procdure and
  measure the bed distance on several points.
  This can take quite a while (10-30 minutes), so grab a coffee and relax.
- After the printer is done, go back to Advanced Settings -> Delta calibration and select "Store settings". This stores all the settings to the internal EEPROM.
  The printer will beep as soon as you hit the button and the settings are saved.
  **This step is important, otherwise the calibration results will be lost after a reboot/power cycle and the auto-leveling needs to be perfomed again!!!**
- If you use the Z probe I recommended above, flap it out of the way.
- Now, go to Menu -> Motion and select "Auto Home". Afterwards, open the "Move Axis" menu and place a small piece of paper on the build plate.
- Set "Soft Endstops" to "Off" and then select Move Z -> Move 10mm. Move the axis to +10 mm by turning the knob.
- Go back and select "Move 1mm" or "Move 0.1mm". Move the Z axis further down until it grabs the paper. You should still be able to move the paper underneath the nozzle, but
  there should be a slight resistance. If you cannot move the paper, move the nozzle back up. If there is no resistance, move the nozzle down until you can feel a resistance.
- Check which position is shown on the display and write it down somewhere. For my printer with the Z probe I recommended above, I got a value of -14.40 mm.
- Go to Menu -> Configuration -> Probe Z Offs. Put the value you wrote down in here.
- Scroll down and select "Store settings". This stores all the settings to the internal EEPROM. The printer will beep as soon as you hit the button and the settings are saved.
  **This step is important, otherwise the Z offset we just configured will be lost after a reboot/power cycle and it needs to be set again!!!**
- Press the reset button on the front of your printer and wait for it to reboot.
- Make sure that the paper is still placed on your buildplate. Go to Menu -> Motion and select "Auto Home". Afterwards, open Move Axis -> Move Z -> Move 10mm and move the axis to 0.0mm
  (it will not let you move it below 0.0mm, so just turn the know until it shows 0.0mm).
  Now the nozzle should be in the same position as earlier, where the paper can be moved with some resistance.
	  
The calibration is now complete and you can start printing. If you have issues with your first layer, the first step would be to adjust the Probe Z offset a bit. If that is not sufficient,
you may need to redo the calibration.
As already mentioned, the stock Z probe gives poor measurement results. I would highly recommend to print the Z probe recommended above to get better results.

## Slicer

The Cura 15.04 that shipped with the Micromake D1 is also extremely outdated (April 2015). Using a modern slicer will give you much more features,
e.g. different print profiles, more advanced speed settings and completely new capabilities, like ironing or arachane slicing. They will also produce much more
optimized G-Code which reduces print time and improves print quality.

I have been using PrusaSlicer for a long time now and I can highly recommend to give it a try for your Micromake. I uploaded my printer profile and print settings 
(see "_PrusaSlicer" folder). To import them to PrusaSlicer, install it first on your PC. After startup, go to Menu -> Help -> Show Configuration Folder (as of V2.8.0).
Then copy all the files from "_PrusaSlicer -> print" (GitHub) to the "print" folder (Configuration folder) and the printer profile from "_PrusaSlicer -> printer"
(GitHub) to the "printer" folder (Configuration folder). After a restart of PrusaSlicer, you can select the printer and the corresponding printer profiles on the
sidebar at the right.

If you do not want to use PrusaSlicer, a current version of Cura (or any other slicer for that matter) should also work, however, I have not tested it.

# Official Marlin README
<p align="center"><img src="buildroot/share/pixmaps/logo/marlin-outrun-nf-500.png" height="250" alt="MarlinFirmware's logo" /></p>

<h1 align="center">Marlin 3D Printer Firmware</h1>

<p align="center">
    <a href="/LICENSE"><img alt="GPL-V3.0 License" src="https://img.shields.io/github/license/marlinfirmware/marlin.svg"></a>
    <a href="https://github.com/MarlinFirmware/Marlin/graphs/contributors"><img alt="Contributors" src="https://img.shields.io/github/contributors/marlinfirmware/marlin.svg"></a>
    <a href="https://github.com/MarlinFirmware/Marlin/releases"><img alt="Last Release Date" src="https://img.shields.io/github/release-date/MarlinFirmware/Marlin"></a>
    <a href="https://github.com/MarlinFirmware/Marlin/actions"><img alt="CI Status" src="https://github.com/MarlinFirmware/Marlin/actions/workflows/test-builds.yml/badge.svg"></a>
    <a href="https://github.com/sponsors/thinkyhead"><img alt="GitHub Sponsors" src="https://img.shields.io/github/sponsors/thinkyhead?color=db61a2"></a>
    <br />
    <a href="https://twitter.com/MarlinFirmware"><img alt="Follow MarlinFirmware on Twitter" src="https://img.shields.io/twitter/follow/MarlinFirmware?style=social&logo=twitter"></a>
</p>

Additional documentation can be found at the [Marlin Home Page](https://marlinfw.org/).
Please test this firmware and let us know if it misbehaves in any way. Volunteers are standing by!

## Marlin 2.1

Marlin 2.1 continues to support both 32-bit ARM and 8-bit AVR boards while adding support for up to 9 coordinated axes and to up to 8 extruders.

Download earlier versions of Marlin on the [Releases page](https://github.com/MarlinFirmware/Marlin/releases).

## Example Configurations

Before building Marlin you'll need to configure it for your specific hardware. Your vendor should have already provided source code with configurations for the installed firmware, but if you ever decide to upgrade you'll need updated configuration files. Marlin users have contributed dozens of tested example configurations to get you started. Visit the [MarlinFirmware/Configurations](https://github.com/MarlinFirmware/Configurations) repository to find the right configuration for your hardware.

## Building Marlin 2.1

To build Marlin 2.1 you'll need [Arduino IDE 1.8.8 or newer](https://www.arduino.cc/en/main/software) or [PlatformIO](http://docs.platformio.org/en/latest/ide.html#platformio-ide). Detailed build and install instructions are posted at:

  - [Installing Marlin (Arduino)](http://marlinfw.org/docs/basics/install_arduino.html)
  - [Installing Marlin (VSCode)](http://marlinfw.org/docs/basics/install_platformio_vscode.html).

### Supported Platforms

  Platform|MCU|Example Boards
  --------|---|-------
  [Arduino AVR](https://www.arduino.cc/)|ATmega|RAMPS, Melzi, RAMBo
  [Teensy++ 2.0](https://www.microchip.com/en-us/product/AT90USB1286)|AT90USB1286|Printrboard
  [Arduino Due](https://www.arduino.cc/en/Guide/ArduinoDue)|SAM3X8E|RAMPS-FD, RADDS, RAMPS4DUE
  [ESP32](https://github.com/espressif/arduino-esp32)|ESP32|FYSETC E4, E4d@BOX, MRR
  [LPC1768](https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/lpc1700-cortex-m3/512-kb-flash-64-kb-sram-ethernet-usb-lqfp100-package:LPC1768FBD100)|ARM® Cortex-M3|MKS SBASE, Re-ARM, Selena Compact
  [LPC1769](https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/lpc1700-cortex-m3/512-kb-flash-64-kb-sram-ethernet-usb-lqfp100-package:LPC1769FBD100)|ARM® Cortex-M3|Smoothieboard, Azteeg X5 mini, TH3D EZBoard
  [STM32F103](https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html)|ARM® Cortex-M3|Malyan M200, GTM32 Pro, MKS Robin, BTT SKR Mini
  [STM32F401](https://www.st.com/en/microcontrollers-microprocessors/stm32f401.html)|ARM® Cortex-M4|ARMED, Rumba32, SKR Pro, Lerdge, FYSETC S6
  [STM32F7x6](https://www.st.com/en/microcontrollers-microprocessors/stm32f7x6.html)|ARM® Cortex-M7|The Borg, RemRam V1
  [SAMD51P20A](https://www.adafruit.com/product/4064)|ARM® Cortex-M4|Adafruit Grand Central M4
  [Teensy 3.5](https://www.pjrc.com/store/teensy35.html)|ARM® Cortex-M4|
  [Teensy 3.6](https://www.pjrc.com/store/teensy36.html)|ARM® Cortex-M4|
  [Teensy 4.0](https://www.pjrc.com/store/teensy40.html)|ARM® Cortex-M7|
  [Teensy 4.1](https://www.pjrc.com/store/teensy41.html)|ARM® Cortex-M7|
  Linux Native|x86/ARM/etc.|Raspberry Pi

## Submitting Patches

- Submit **Bug Fixes** as Pull Requests to the ([bugfix-2.1.x](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.1.x)) branch.
- Follow the [Coding Standards](http://marlinfw.org/docs/development/coding_standards.html) to gain points with the maintainers.
- Please submit your questions and concerns to the [Issue Queue](https://github.com/MarlinFirmware/Marlin/issues).

## Marlin Support

The Issue Queue is reserved for Bug Reports and Feature Requests. To get help with configuration and troubleshooting, please use the following resources:

- [Marlin Documentation](https://marlinfw.org) - Official Marlin documentation
- [Marlin Discord](https://discord.gg/n5NJ59y) - Discuss issues with Marlin users and developers
- Facebook Group ["Marlin Firmware"](https://www.facebook.com/groups/1049718498464482/)
- RepRap.org [Marlin Forum](https://forums.reprap.org/list.php?415)
- Facebook Group ["Marlin Firmware for 3D Printers"](https://www.facebook.com/groups/3Dtechtalk/)
- [Marlin Configuration](https://www.youtube.com/results?search_query=marlin+configuration) on YouTube

## Contributors

Marlin is constantly improving thanks to a huge number of contributors from all over the world bringing their specialties and talents. Huge thanks are due to [all the contributors](https://github.com/MarlinFirmware/Marlin/graphs/contributors) who regularly patch up bugs, help direct traffic, and basically keep Marlin from falling apart. Marlin's continued existence would not be possible without them.

## Administration

Regular users can open and close their own issues, but only the administrators can do project-related things like add labels, merge changes, set milestones, and kick trolls. The current Marlin admin team consists of:

 - Scott Lahteine [[@thinkyhead](https://github.com/thinkyhead)] - USA - Project Maintainer &nbsp; [💸 Donate](https://www.thinkyhead.com/donate-to-marlin)
 - Roxanne Neufeld [[@Roxy-3D](https://github.com/Roxy-3D)] - USA
 - Keith Bennett [[@thisiskeithb](https://github.com/thisiskeithb)] - USA &nbsp; [💸 Donate](https://github.com/sponsors/thisiskeithb)
 - Peter Ellens [[@ellensp](https://github.com/ellensp)] - New Zealand
 - Victor Oliveira [[@rhapsodyv](https://github.com/rhapsodyv)] - Brazil
 - Chris Pepper [[@p3p](https://github.com/p3p)] - UK
 - Jason Smith [[@sjasonsmith](https://github.com/sjasonsmith)] - USA
 - Luu Lac [[@shitcreek](https://github.com/shitcreek)] - USA
 - Bob Kuhn [[@Bob-the-Kuhn](https://github.com/Bob-the-Kuhn)] - USA
 - Erik van der Zalm [[@ErikZalm](https://github.com/ErikZalm)] - Netherlands &nbsp; [💸 Donate](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software)

## License

Marlin is published under the [GPL license](/LICENSE) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.
