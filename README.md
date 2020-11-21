
# apa102 controller

EAGLE schematic and board for apa102 controller

***
## the board

Top:

![SVG of PCB top](./gerblook/b/top.svg)

Bottom:

![SVG of PCB bottom](./gerblook/b/bottom.svg)
***

## the schematic

#### Power:

DC 5V, AMS1117-3V3, RT9193GB-33.

There is no onboard 5V voltage regulator. Power is supplied by an external switched power supply at 5V with a 2.1mm center positive jack, in a common wall-wart or laptop brick form factor. Two options are available for providing power: Space for a standard rectangular PCB-mount DC jack exists and optionally, to keep those milled slots clean, two holes labelled "red" and "black" are available for use with circular panel mount DC jacks. A fuse follows the voltage input connectors and a second fuse sits between the regular 5V on the PCB and the 5V supplied to the LED connector. Two big electrolytic capacitors sit after the 5V in and before the MCU side and LED side of the power to hopefully prevent brownouts if a strip suddenly changes from off to fully illuminated.

When connected to external USB, the 5V-USB, after a fuse, powers the onboard atmega88p USBASP and FT230X serial directly and runs to an AMS1117 3.3v voltage regulator that serves the USB Hub, CP2112 i2c, and the USB enable on the uBlox GPS. The 3V3 output has the datasheet suggested 22uF capacitor.

The uBlox GPS is powered by an RT9193 3.3v voltage regulator. The enable pin of this device is connected to the primary MCU on pin D4 so it can be turned off in software. Alternatively, a solder jumper permits connecting the pin directly to the primary 5V. The RT9193 has the datasheet recommended 22nF capacitor on the bypass pin for low-noise operation and the 3V3 output passes though a ferrite bead and tantalum capacitor on its way to the integrated circuit.

#### GPS:

A standard uBlox NEO 7M occupies the upper right quadrant of the board. The numerous vias and general layout are hopefully compliant with datasheet requirements. The PPS and TX pins connect to the MCU with RX left disconnected. There is board space for an external eeprom, a cheap and simple part, but it is generally not required.

A CR1220 battery is dedicated to the GPS to enable warm starts. An SMA connection reminds us an external active antenna is required. While the mini-USB connector is nearby, it is not directly connected to the GPS.

#### USB-i2c:

The bridge the gap between computers and the hardware on the circuit board, a CP2112 offers access to the i2c bus.

#### RTC:

Preserving time across reboots, an MCP79408 or MCP79412 is set and periodically synchronized by the GPS. To decouple GPS time and RTC time, the RTC has its own battery so that even if GPS precision is delayed because of a dead-battery cold start, hopefully the RTC has a good idea of at least what the current minute is.

#### MCU:

Basically equivalent in function to an Arduino Nano, the heart of the board is its ATMega328p. It controls the input and output and all those things.

The chip runs at 16MHz with an external crystal. The MOSI and SCK pins are protected with 220 Ohm PTC fuses and 5.1V Zener diodes in the style of Ruggeduino.

Refer to the example program for further details.

#### ISP:

For convenience, an onboard USBASP programmer exists. During assembly and initial configuration be mindful of the solder jumpers on the underside of the PCB connected to the RESET pin of the 6-pin IDC ISP header and the two pins that when jumpered allow USB power to flow to the atmega328p target.

#### USB Hub:

After the board added a fourth USB connector, it became time to just add a hub, the Texas Instruments TUSB2046B. 27 Ohm resistors should be present in the correct places and absent where appropriate. A 1.5k Ohm resistor is connected between USB-3V3 and the external D+ line to signal the device as high speed USB. There is a MAX809S attached on the reset pin to provide the necessary pulse at startup.

#### USB Serial:

A cheaper alternative to the common FT232RL at half the price this board has a FT230X for simple debugging with serial print statements from the MCU. All data pins other than Transmit-Out are disconnected.

#### Connections:

Header for LEDs. Batteries. ISP. USB.

#### Switch Debouncing:

normal hex inverter

#### Input:

ALPS trimpot, ALPS trimpot with switch

#### Temp Sensor:

MCP9808

***

