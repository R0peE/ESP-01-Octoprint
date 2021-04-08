# esp-01-octoprint

The intention of this short guide is, how to connect Octoprint via WiFi to popular BigTreeTech, or other boards which have serial connection support, as I'm a cheap ass and don't want to buy a RaspberryPi, because I have a NAS running constantly at home :). In my example I will show how to set this up on the all time favourite Ender-3 Pro and SKR Mini v1.2.

Your setup my vary though but the basic concepts will be the same. Generally all popular 3D printer boards will have a pinout diagram somewhere, you can use that for reference to map out your pins for serial connection. (This is also possible on RAMPS too ;))

## Required hardware
- A printer :)
- SKR Mini v1.2
- BTT ESP-01s (Or any generic ESP-01s with enough flash space)

## Required software
- Octoprint: https://octoprint.org/
- Octoprint network printing plugin: https://github.com/hellerbarde/OctoPrint-Network-Printing
- Proper Marlin configuration (serial ports and baud rates set up)
- ESP3D Firmware: https://github.com/luc-github/ESP3D

## Hardware setup

In my example I will walk through on how to set this up on the SKR mini v1.2. Keep in mind, this will also work exactly the same on the v2.0 also, as the board layout is completely identical.

For this you will have to sacrifice your TFT pins, if you don't have a BTT TFT already installed (in that case you can connect the ESP-01s to that directly without any fancy stuff).

The wiring you will see on the image attached to this repositroy (esp-01s-skr-mini-wiring.png). There will be one confusing part though, to make the serial communication work between the SKR board and the ESP-01s the RX and TX pins have to be connected vica versa: RX to TX and TX to RX. The ESP-01s will require 3.3V!!! to operate, so watch out not to accidentally connect it to any 5V pin, as it can burn it no issue. This is why we will use the 3.3V pin from the SWD pins (it is also not commonly used).

After you are finished with the wiring, and start your printer, on your LCD the ESP-01s will write you the IP address which you can connect to. At first it will run in accesspoint mode, but later that can be changed while you are doing it's initial setup.

## Software setup
Preparing Marlin should not be an issue, for me out of the box it was working with the SKR mini v1.2 without any modification, but here is my code snippet from config.h as a reference:

```
/**
 * Select the serial port on the board to use for communication with the host.
 * This allows the connection of wireless adapters (for instance) to non-default port pins.
 * Serial port -1 is the USB emulated serial port, if available.
 * Note: The first serial port (-1 or 0) will always be used by the Arduino bootloader.
 *
 * :[-1, 0, 1, 2, 3, 4, 5, 6, 7]
 */
#define SERIAL_PORT 2

/**
 * Select a secondary serial port on the board to use for communication with the host.
 * :[-1, 0, 1, 2, 3, 4, 5, 6, 7]
 */
#define SERIAL_PORT_2 -1

/**
 * This setting determines the communication speed of the printer.
 *
 * 250000 works in most cases, but you might try a lower speed if
 * you commonly experience drop-outs during host printing.
 * You may try up to 1000000 to speed up SD file transfer.
 *
 * :[2400, 9600, 19200, 38400, 57600, 115200, 250000, 500000, 1000000]
 */
#define BAUDRATE 115200
```

If you purchase a BTT ESP-01s board, it will most probably come with ESP3D preinstalled. If that is not case case please refer to the link above to Luc's github, there will be a detailed guide on how to install the firmware, connect to it via WiFi, and upload the webui. When you are setting it up, and if you modify the Data port, write it down because we will need it later, but by default it should be 8888. 

For Octoprint you will have to install the Octoprint network plugin, the simplest is via the plugin manager: OctoPrint Settings -> Plugin Manager -> Get More -> ... from URL paste: https://github.com/hellerbarde/OctoPrint-Network-Printing/archive/main.zip -> Install
After the install is complete, it will be required to restart Octoprint.

After the restart, we can add the ESP-01s Data port as an additional serial port in Octoprint: OctoPrint Settings -> Serial Connection -> Additional serial ports -> paste the following: `socket://<your esp-01s ip>:8888` -> Save

If everything goes well you will be able to select on the Octoprint main screen under Serial port dropdown your newly added ip and port. The baud rate can stay on AUTO but you can also choose different one in case the connection is instable.

When you press the connect button, Octoprint will be able to connect to your printer via a WiFi connection :)

## Final thoughts
As always, i'm not responsible if you mess things up on your machine. The setup ofcourse is not the most elegant thing how you can achieve Octoprint, but if you are on a budget and like to tinker, and your WiFi is stable enough, this is a pretty good solution. I have not experienced any issues whatsoever since I'm running this setup. Some boards like the SKR 1.4 and Turbo will have a dedicated port for the ESP-01s similar to the BTT TFT's, over there this setup is essentially plug and play.

Hope you have learned something from this, and maybe I have made your 3D printing experience more comfortable :) Cheers!



