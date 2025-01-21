---
date: '2025-01-20T15:12:09+05:00'
title: 'How to Connect a Raspberry PI 5 to USB TTY Cable'
---

Connecting a Raspberry Pi 5 to a USB TTY cable is a common way to interact with it through a serial connection, especially for debugging or setting up the device without using a display.

**Prerequists**
1. Raspberry Pi 5.
2. USB TTY (serial) cable.
3. Computer with a terminal emulator (minicom/screen).
4. GPIO pinout diagram of Raspberry Pi 5 (for reference).
4. Power source for Raspberry Pi (optional if USB TTY can power it, though not recommended).

### **Before Starting!**

### **[Issuse with firmware](https://forums.raspberrypi.com/viewtopic.php?t=361397#p2171244)**
 

UART does NOT work on the RPI5 from the factory. We will need a firmware update to fix this that prevents the dtoverlays for UARTs from working.

- Install rpi-update with the following commands:

      > sudo curl -L --output /usr/bin/rpi-update https://raw.githubusercontent.com/Hexxeh/rpi-update/master/rpi-update && sudo chmod +x /usr/bin/rpi-update

- Then update the firmware on your RPI5 with:

        > sudo rpi-update

### **Enable UART**

To manually configure UART, you can edit the config.txt file.
>Edit */boot/firmware/config.txt* and add:

    > enable_uart=1     


### **How to Connect**

- Locate the GPIO Pins
- Find the GPIO header on the Raspberry Pi 5.
- Identify the following pins:

![USBTTY](/rpiotty.png#center)

**GND (Ground): Usually black wire on the USB TTY cable.
TX (Transmit): Sends data from the Pi to the computer.
RX (Receive): Receives data from the computer to the Pi.**

Use a GPIO pinout chart to locate these pins. For Raspberry Pi 5, it will likely be similar to previous models.
![GPIO Pinout](/RPI5_PINOUT.png#center)

### **Making connections**

You will need to connect:

- **GND** with **Ground** - **Pin# 06**
- **TX** with **GPIO14** - **Pin# 08**
- **RX** with **GPIO15** - **Pin# 10**

![GPIO Connection](/hpio.png#center)
 
**Plug the USB TTY Cable into the Computer**

- Insert the USB end of the TTY cable into your computer.
- The cable will create a virtual COM port (e.g /dev/ttyUSB0).

**Configure and Access Serial Console**

- Open a terminal.
- Identify the port with:

        > ls /dev/ttyUSB*

- Use a terminal emulator like screen or minicom to connect:

        > screen /dev/ttyUSB0 115200

*Replace /dev/ttyUSB0 with the actual port name.

- Turn on the Raspberry Pi.
- If everything is set up correctly, you should see boot messages in the terminal.
- Log in to the Pi using the default username (pi) and password (raspberry), or your custom credentials.


**You should see something similar to this.**

![GPIO Pinout](/pic-selected-250120-1633-59.png)

**This is it! You have done it. Congrats!**



