# CH552G MacroPad plus
The MacroPad plus is a versatile extension for your keyboard that has six programmable keys and a rotary encoder knob. These buttons and the knob can be configured to perform a variety of actions, such as triggering specific key combinations, mouse movements, and game controller inputs. You can easily modify the firmware used to operate the MacroPad to suit your needs.

Each button and knob on the MacroPad also has addressable LEDs (NeoPixels), which can be programmed in the same way as the keys. This lets you customize the colors, patterns, or animations of your device.

Connecting the MacroPad to your PC is easy - simply plug it in via USB. It's immediately recognized by your PC as an HID composite device, so no special drivers are needed. This makes the MacroPad a user-friendly and accessible option for anyone looking to enhance their keyboard's capabilities.

![MacroPad_plus_pic1.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic1.jpg)

# Hardware
## Schematic
![MacroPad_plus_wiring.png](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_wiring.png)

## CH552G 8-bit USB Device Microcontroller
The CH552G is a low-cost, enhanced E8051 core microcontroller compatible with the MCS51 instruction set. It has an integrated USB 2.0 controller with full-speed data transfer (12 Mbit/s) and supports up to 64 byte data packets with integrated FIFO and direct memory access (DMA). The CH552G has a factory built-in bootloader so firmware can be uploaded directly via USB without the need for an additional programming device.

## Buildung Instructions
1. Take the Gerber files and send them to a PCB manufacturer of your choosing. They will use these files to create the circuit board for your device.
2. Once you have the PCB, you can start soldering the components onto it. Use the BOM (bill of materials) and schematic as a guide to make sure everything is connected correctly.
3. Upload the firmware by following the instructions in the next section (see below).
4. To create the case for your device, use the STL files with your 3D printer. Make sure to use transparent filament for the keycaps, knob, and ring.
5. After printing, secure the PCB to the bottom of the case using three self-tapping M2x5mm screws.
6. Next, glue the ring from the bottom into the circular recess in the top of the case.
7. Finally, assemble the case. Place the keycaps onto the switches and the knob onto the rotary encoder. Your device is now ready to use!

![MacroPad_plus_pic2.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic2.jpg)
![MacroPad_plus_pic3.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic3.jpg)
![MacroPad_plus_pic5.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic5.jpg)
![MacroPad_plus_pic6.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic6.jpg)
![MacroPad_plus_pic7.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic7.jpg)

# Modifying, Compiling and Installing Firmware
## Customizing the Firmware
The definition of the macros and their assignment to individual key events is done by adjusting the firmware accordingly, which allows maximum freedom and flexibility. To do this, open the macropad_plus.c file and edit the section with the macro functions. The source code is commented in such a way that it should be possible to make adjustments even with basic programming skills.

## Preparing the CH55x Bootloader
### Installing Drivers for the CH55x Bootloader
If you're using Linux, you don't need to install a driver. However, Linux may not give you enough permission by default to upload your code with the USB bootloader. To resolve this issue, you can open a terminal and enter the following commands:

```
echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="4348", ATTR{idProduct}=="55e0", MODE="666"' | sudo tee /etc/udev/rules.d/99-ch55x.rules
sudo service udev restart
```

If you're using Windows, you'll need to install the CH372 driver, which you can download from this [link](http://www.wch-ic.com/downloads/CH372DRV_EXE.html). Alternatively, you can use the [Zadig Tool](https://zadig.akeo.ie/) to install the correct driver. Simply click on "Options" and "List All Devices" to select the USB module, and then install the libusb-win32 driver. To do this, make sure the board is connected and the CH55x is in bootloader mode.

### Entering CH55x Bootloader Mode
When you connect a brand new chip to your PC via USB, it automatically starts in bootloader mode. However, after firmware has been uploaded, the bootloader must be started manually for new uploads. To do this, you need to disconnect the board from the USB port and all voltage sources, then press and hold the BOOT button while reconnecting the board to the USB port of your PC. The chip will start again in bootloader mode, and you can release the BOOT button and upload new firmware within a few seconds.

Once the MacroPad firmware is installed, you can enter the bootloader by holding down the rotary encoder switch while connecting the device to the USB port. This way, you don't need to open the case to install new firmware. All NeoPixels will light up white while the device is in bootloader mode, which lasts for about 10 seconds.

## Compiling and Uploading using the makefile
### Installing SDCC Toolchain for CH55x
Install the [SDCC Compiler](https://sdcc.sourceforge.net/). In order for the programming tool to work, Python3 must be installed on your system. To do this, follow these [instructions](https://www.pythontutorial.net/getting-started/install-python/). In addition [pyusb](https://github.com/pyusb/pyusb) must be installed. On Linux (Debian-based), all of this can be done with the following commands:

```
sudo apt install build-essential sdcc python3 python3-pip
sudo pip install pyusb
```

### Compiling and Uploading Firmware
- Open a terminal.
- Navigate to the folder with the makefile. 
- Connect the board and make sure the CH55x is in bootloader mode. 
- Run ```make flash``` to compile and upload the firmware. 
- If you don't want to compile the firmware yourself, you can also upload the precompiled binary. To do this, just run ```python3 ./tools/chprog.py macropad_plus.bin```.

## Compiling and Uploading using the Arduino IDE
### Installing the Arduino IDE and CH55xduino
Install the [Arduino IDE](https://www.arduino.cc/en/software) if you haven't already. Install the [CH55xduino](https://github.com/DeqingSun/ch55xduino) package by following the instructions on the website.

### Compiling and Uploading Firmware
- Copy the .ino and .c files as well as the /src folder together into one folder and name it like the .ino file. 
- Open the .ino file in the Arduino IDE.
- Go to **Tools -> Board -> CH55x Boards** and select **CH552 Board**.
- Go to **Tools** and choose the following board options:
  - **Clock Source:**   16 MHz (internal)
  - **Upload Method:**  USB
  - **USB Settings:**   USER CODE /w 266B USB RAM
- Connect the board and make sure the CH55x is in bootloader mode. 
- Click **Upload**.

# References, Links and Notes
1. [EasyEDA Design Files](https://oshwlab.com/wagiminator)
2. [CH551/552 Datasheet](http://www.wch-ic.com/downloads/CH552DS1_PDF.html)
3. [SDCC Compiler](https://sdcc.sourceforge.net/)
4. [MacroPad mini](https://github.com/wagiminator/CH552-Macropad-mini)
5. [USB Knob](https://github.com/wagiminator/CH552-USB-Knob)

![MacroPad_plus_pic8.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic8.jpg)
![MacroPad_plus_pic9.jpg](https://raw.githubusercontent.com/wagiminator/CH552-MacroPad-plus/main/documentation/MacroPad_plus_pic9.jpg)

# License
![license.png](https://i.creativecommons.org/l/by-sa/3.0/88x31.png)

This work is licensed under Creative Commons Attribution-ShareAlike 3.0 Unported License. 
(http://creativecommons.org/licenses/by-sa/3.0/)
