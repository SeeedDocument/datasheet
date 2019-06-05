# Raspberry Pi Relay Board v1.0 SKU:103030029


![](https://raw.githubusercontent.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/master/img/Raspberry_Pi_Relay_Board_v1.0.jpg)

The Relay Shield utilizes four high quality relays and provides NO/NC interfaces that control the load of high current. Which means it could be a nice solution for controlling devices that couldn’t be directly controlled by IIC bus. Standardized shield form factor enables smoothly connection with the Raspberry Pi. The shield also has four dynamic indicators show the on/off state of each relay.



Version
--------

| Product Version              | Changes                                   | Released Date |
|------------------------------|-------------------------------------------|---------------|
| Raspberry Pi Relay Board v1  | Initial                                   | Mar 2015     |


## Features

-   Raspberry Pi compatible
-   Interface: IIC, Three hardware SW1 (1, 2, 3) select the fixed I2C-bus address
-   Relay screw terminals
-   Standardized shield shape and design
-   LED working status indicators for each relay
-   COM, NO (Normally Open), and NC (Normally Closed) relay pins for each relay
-   High quality relays
-   Working status indicators for each relay

## Specifications

| Parameter             | Value/Range    |
|-----------------------|----------------|
| Operating voltage     | 4.75 to 5.5V   |
|Working Current	      |10 to 360mA  |
| Switching Voltage     | 30VDC/250VAC    |
| Switching Current | 15A  |
| Switching Power       | 2770VA/240W    |
| Relay Life      | more than 100,000cycle   |
|Main Chip|PCAL9535A|
| Size                  | L:130mm W:60mm H:32mm     |
| Weight                |  59.4g           |
|Package size|L: 135mm W: 65mm H: 33mm|
|Gross Weight|79g|


## Hardware Overview

![](https://raw.githubusercontent.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/master/img/Raspberry_Pi_Relay_Board_v1.0_p3.jpg)

## Usage


This section was written by John M. Wargo, here we would like to express our gratitude to John's contribution. We have amended the original text a little to fit it in the whole Seeed's document. Please click [here](http://johnwargo.com/microcontrollers-single-board-computers/using-the-seeed-studio-raspberry-pi-relay-board.html) to visit the original document on his website.

The steps for installing the board and verifying that it works includes the following steps:

- Step1.  Mount the Relay board on the Raspberry Pi
- Step2. Enable the Raspbian I2C software interface
- Step3. Validate that the Raspberry Pi recognizes the board
- Step4. Run some Python code to exercise the board

### Step1. Mounting the Relay Board

Mounting the board is easy, it comes with the appropriate female headers you need to mount it on any Raspberry Pi board with male headers. Note: You’ll have to add male headers to the Raspberry Pi Zero to use the board.

We recommend you putting some electrical tape on top of the Raspberry Pi Ethernet port before mounting the board. If you mount the board without using standoffs (as I’ve done in the example figure below), there’s a chance the board will make contact with the Ethernet port housing and cause a problem.

![](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/raw/master/img2/seed-figure-01.png)
**Figure 1**

For a production project, We’d definitely recommend using standoffs to hold the two boards in place.

The relay board is configured for an older Raspberry Pi with a 26 pin header, so when you connected it to a Raspberry Pi with 40 pin headers, you’ll need to shift it all the way to the side like We’ve shown in the figure. If you don’t align the pins correctly, you’ll have problems later as it simply won’t work.


### Enabling I2C
The relay board communicates with the Raspberry Pi through an I2C interface [https://en.wikipedia.org/wiki/I%C2%B2C](https://en.wikipedia.org/wiki/I%C2%B2C). This interface is disabled by default in the Pi’s Raspbian OS, so you’ll have to turn it on before you can use the board. Power up the Pi and let it boot to the graphical interface. When it’s up and running, open the Pi menu, select Preferences, then Raspberry Pi Configuration as shown in the following figure:

![](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/raw/master/img2/seed-figure-02.png)
**Figure 2**

In the window that opens, select the Interfaces tab as shown in the following figure. Enable the option next to I2C as shown in the figure and click the OK button to continue. When you reboot the PC, the Pi should see the relay board. In the next section, we’ll verify that the Pi sees the relay board.

![](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/raw/master/img2/seed-figure-03.png)
**Figure 3**

### Validating the Raspberry Pi Sees the Relay Board

With the I2C interface enabled, it’s time to make sure the Raspberry Pi sees the relay board. Open a terminal window on the Pi and execute the following command:
```
i2cdetect -y -r 1
```
The application will display a dump of the recognized I2C devices as shown in the following figure. In this example, there’s only one I2C board on the system, the relay board configured at an address of 20. You’ll see how this value is important later in this article.

![](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/raw/master/img2/seed-figure-04.png)
**Figure 4**

You’re supposed to be able to use switches on the relay board to set the I2C address, there are 4 DIP switches on the board, let’s see what happens when you change them.

There are four switches, three labeled A0 through A2, and one labeled NC. The NC means No Connection. Each switch has a high and a low setting, so the following table will lay out how to use them to set an I2C address for the board:

|A0| A1 |A2 |Address|
|---|---|---|---|
|High|High|High|20|
|Low|High|High|21|
|High|Low|High|22|
|High|High|Low|24|
|High|Low|Low|26|
|Low|Low|Low|27|

### Running the Test Application

Please use the test code from [github repository](https://github.com/johnwargo/Seed-Studio-Relay-Board). Grab the code from there and you’ll be able to easily complete the following step.

To run the test application, open a terminal window, navigate to where you’ve extracted the sample application and run the application using the following command:

```
python ./seeed_relay_test.py
```
![](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/raw/master/img2/seed-figure-05.png)
**Figure 4**

When prompted for input, you’ll type commands to turn the relays on and off:

- Typing 1on, 2on, 3on, or 4on and pressing enter will cause the specified relay to turn on.
- Typing 1off, 2off, 3off, or 4off and pressing enter will cause the specified relay to turn off
- Typing allon or alloff will turn all relays on or off.


### Using The Python Module
To use the module in your own Python applications, copy the module (relay_lib_seeed.py) into your project folder, then import the module in your Python application by adding the following line to the beginning of your application:

>from relay_lib_seeed import *

This exposes a series of functions to your application:

- relay_on(int_value) - Turns a single relay on. Pass an integer value between 1 and 4 (inclusive) to the function to specify the relay you wish to turn on. For example: relay_on(1) will turn the first relay (which is actually relay 0 internally) on.
- relay_off(int_value) - Turns a single relay on. Pass an integer value between 1 and 4 (inclusive) to the function to specify the relay you wish to turn on. For example: relay_on(4) will turn the first relay (which is actually relay 3 internally) off.
- relay_all_on() - Turns all of the relays on simultaneously.
- relay_all_off() - Turns all of the relays off simultaneously.

The module exposes a configuration value you will want to keep in mind as you work with the board:
```
# 7 bit address (will be left shifted to add the read write bit)
DEVICE_ADDRESS = 0x20
```

Remember that value? 20? The board defaults to this address. If you change the switches on the board, you will need to update this variable accordingly.

To see the module in action, open a terminal window on the Raspberry Pi, navigate to the folder where you extracted this repository's files, and execute the following command:
```
python ./relay_lib_seeed_test.py
```
The application will:

- Turn all of the relays on for a second
- Turn all of the relays off
- Cycle through each of the relays (1 through 4) turning each on for a second

The module will write indicators to the console as it performs each step as shown in the following figure:

![](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/raw/master/img2/seed-figure-06.png)
**Figure 6**

LEDs on the relay board (one for each relay) will illuminate when the relays come one. On my board, they weren't in sequence, so don't expect them to light in order.

The code that does all this looks like the following:

```
# turn all of the relays on
relay_all_on()
# wait a second
time.sleep(1)
# turn all of the relays off
relay_all_off()
# wait a second
time.sleep(1)
# now cycle each relay every second in an infinite loop
while True:
for i in range(1, 5):
   relay_on(i)
   time.sleep(1)
   relay_off(i)
```

That’s it, that’s all there is to it. Enjoy.

Resources
---------
- [Raspberry_Pi_Relay_Board_v1.0_sch_pcb](https://raw.githubusercontent.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/master/res/Raspberry_Pi_Relay_Board_v1.0_sch_pcb.zip)
- [Raspberry_Pi_Relay_Board_v1.0_PDF](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/raw/master/res/Raspberry%20Pi%20Relay%20Board%20v1.0.pdf)
- [HLS8L Datasheet](https://raw.githubusercontent.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/master/res/HLS8L.pdf)
- [PCAL9535A Datasheet](https://raw.githubusercontent.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/master/res/PCAL9535A.pdf)
-  [Python Test Code](https://github.com/johnwargo/Seed-Studio-Relay-Board)
-  [C# Test Code](https://github.com/SeeedDocument/Raspberry_Pi_Relay_Board_v1.0/tree/master/res/RPiRelayBoard)


<!-- This Markdown file was created from http://www.seeedstudio.com/wiki/Raspberry_Pi_Relay_Board_v1.0 -->

## Tech Support
Please submit any technical issue into our [forum](http://forum.seeedstudio.com/) or drop mail to techsupport@seeed.cc. 