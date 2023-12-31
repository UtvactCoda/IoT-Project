# IoT Project Tutorial – Project: Weather Data System
Joakim Rytterlöv, jr223cs


# Project: Weather Data System
This is a tutorial on how to build, configure and run a custom weather data system. The devices used for gathering temperature readings are three Raspberry Pi Pico WH with the MicroPython firmware installed. The full requirements list for both hardware and software can be found below in the section “_Requirements / Materials_”.


# Background
This project has been built to try to solve a real-world problem (although a rather mundane one at that), where I’ve created a system that keeps track of the outdoor and indoor temperatures. The temperatures are then sent to a custom application that processes the data and performs different so called “_flows_” which will be described later. What this ultimately means, is that the system reads and compares the incoming temperature values and decides to do multiple things with the information in different parallel processes:

1. The data is saved in a database, with the corresponding identifying information (sensor **ID**), along with the recorded data regarding both the measured **temperature** and the **time** it was measured.
2. The data is visualized on a separate dashboard which is updated live when new incoming data arrives in the system.
3. The same data is also processed further: the measurements are compared with each other to find out which sensor has the lowest measured temperature. The result is then passed on to the dashboard as well as to a separate LCD display (see the _Hardware_ section below). The idea is that every time the sensor with the lowest reading changes, a notification is sent to the user via the dashboard and via the LCD display.

The estimated time to complete this tutorial from start to finish is between 1 to 3 hours, depending on how much you’ve previously worked with programming, electronics and the Internet of Things (hereon referred to as _IoT_). It’s a somewhat complex project and therefore needs a lot of work to set up.


# Objective
The reason why I chose this project is because it has been a great challenge to put my knowledge to test due to its complexity. It has also served as an introduction for getting started with IoT, embedded systems, and electronics. While working with the project, it has also sparked a new interest in working with hardware (pun intended). The outcomes from this project have been first and foremost, to learn new things and to build physical devices that run code on them which combines previous knowledge about programming, with new knowledge about physical computing and IoT.

Even if this project isn’t ready for selling in stores, I think that the idea could serve as a starting point in a bigger eco-system of smart devices that help users in their everyday lives by solving recurring problems in a more efficient manner. The insights from this project’s data can be used in various ways, such as discovering at which times it's the "best" to open the windows, and when it is "best" to keep them closed. This helps to achieve a maximized comfort of the indoor climate. It also serves a a "generic" thermometer. Since the project utilizes Node-Red, it is rather trivial to set up new functionality, such as using LED indicators for different ranges of temperatures, for example to be used as a "recommendation system" for which outfits to use, depending on the measured temperature, e.g., 25 celsius degrees means t-shirt, 10 celsius degrees means coat.

The data received from the devices tell us the measured **temperature** and at what **time**, and by incorporating the data into a graph in the dashboard, it is possible to see when temperatures intersect throughout the day. This could possibly help detect weather patterns, or it could serve as a “cheatsheet” for which times it is best to open the windows or not – since opening the windows when it’s warmer outside just make the indoor climate worse for productivity.



# Requirements / Materials

## Hardware
**Note:** I have links to the stores “_Electrokit_” and “_Kjell_”, but I am in no way affiliated with them; it’s simply where I purchased the items, so feel free to use any other online store to purchase your own parts. Just make sure that you buy the same – or at least equivalent items if you want your project to function identically to my setup. Sometimes you can find datasheets on the product reseller’s website where you can find serial numbers and EAN numbers etc. which you can use to determine if you found a perfect match.

### What to buy
**Note:** I write the cost of the items in Swedish currency (SEK).  
**Note:** I write the **price per item**, not the final price of multiple items.

3 x Raspberry Pi Pico WH  
**What it is:** It’s a microcontroller that you can use to write programs that controls e.g., lights or motors, but it can also read sensor data – such as in this case where it reads the temperature by using temperature sensors.  
**Price:** 109.00 SEK.  
https://www.electrokit.com/produkt/raspberry-pi-pico-wh/

1 x Pimoroni Pico Display Pack 2.0  
**What it is:** It’s an LCD display that you connect to a Raspberry Pi Pico WH so that you can display text, shapes, and images. For this project it presents data values.  
**Price:** 329.00 SEK  
https://www.electrokit.com/produkt/pico-display-pack-2-0/

2 x Solderless Breadboards. Minimum 400 Pin recommended.  
**What it is:** It’s a block of plastic with a system of holes with metals inside them that are organized in a systematic way which make it easy to connect and test electrical circuits.  

You can choose any combinations of pins, as long as the components fit (see the schematics in the section _Schematics_ below to decide for yourself how many pins you need). The breadboards I used for this project are the following:  

1 x Breadboard 840 Pin  
**Price:** 69.00 SEK  
https://www.electrokit.com/produkt/kopplingsdack-840-anslutningar/

1 x Breadboard 400 Pin  
**Price:** 49.00 SEK  
https://www.electrokit.com/produkt/kopplingsdack-400-anslutningar/

3 x USB Cable: A to Micro B connection  
**What it is:** It is used to connect the Raspberry Pi Pico WH to your computer so you can save and run code on it.  
**Price:** 39.00 SEK  
https://www.electrokit.com/produkt/usb-kabel-a-hane-micro-b-5p-hane-1-8m/

2 x Raspberry Pi 5.1V 2.5A power supply with micro-USB connection  
**What it is:** It allows you to power your Raspberry Pi Pico WH from a wall socket, as opposed to having it connected to the PC. This enables you to move your hardware to another room and still be able to run the code on the Pico.  
**Price:** 229.00 SEK  
https://www.electrokit.com/produkt/batterieliminator-5-1v-2-5a-microusb-rpi-vit/

1 x Various assortment of jumper wires / Dupont cables (Male-to-Male connections)  
**What it is:** It is used to connect hardware (e.g., LEDs) using the connections on the breadboard.  
**Price:** 49.00 SEK  
https://www.electrokit.com/produkt/labbsladd-40-pin-30cm-hane-hane/

1 x Various assortment of jumper wires / Dupont cables (Female-to-Male connections)  
**What it is:** It is used to connect hardware that can't be connected to the breadboard directly, but instead have pins you connect to the breadboard. In this case, it is used for the DHT11 sensor (a temperature & humidity sensor).  
**Price:** 49.00 SEK  
https://www.electrokit.com/produkt/labbsladd-40-pin-30cm-hona-hane/

4 x Resistors  
**What it is:** It adds more electrical resistance to a circuit which lowers the current running in the circuit. In other words, applying a bigger resistance value means that for example LEDs get lower brightness. If a high enough value is used, the light won’t even turn on!  
They are often sold in bulk — below is a kit with a varied assortment of resistor values (1460-pack).  

**Additional information**  
Resistance, or _Ohms_ as it is called in “electronics language”, is often expressed using the symbol: Ω (the uppercase Greek letter Omega). You will often see this when working with resistors, as their values are usually expressed this way. For example:  
- 220 Ohms
- 220 Ω  
both mean the same thing.  

The resistor values have been calculated based on the 5mm LED specifications on Kjell's product webpage below:  
Red, green, yellow at 10 mA: 1,95-2,05 V  
Red, green, yellow at 20 mA: 2,01-2,10 V  
White, blue at 10 mA: 2,87-2,94 V  
White, blue at 20 mA: 3,01-3,11 V  

By using Ohm's law, we can calculate the resistor values using the following formula:  
Resistance = Voltage / Current  
Or as it's more known as: R = V / I  
Resistance is the resistor value to use, Voltage is the Pico's 3.3V output and the Current is either 10 mA or 20 mA depending on which LED we use and the desired effect.
In this project I went with 20 mA, hence the "0.02" in the calculations below.  

**Substituting the variables with the real values gives**  
Red LED = (3.3 - 2.01) / 0.02 = 64.5, rounded up to closest resistor value = 68Ω  
Yellow LED = (3.3 - 2.01) / 0.02 = 64.5, rounded up to the closest resistor value = 68Ω  
Blue LED = (3.3 - 3.01) / 0.02 = 14.5, rounded up to the closest resistor value = 15Ω.  

The reason why we round UP instead of down, is because if there is too little resistance, we risk burning our LEDs as they will then get too high current in the circuit.  

**For this project you’ll need**  
2 x 68 Ω resistor (red and yellow LED)  
1 x 15 Ω resistor (blue LED)  
1 x 4.7k Ω resistor (used as a pull-up resistor for the sensor: DS18B20)  

**IMPORTANT:** I wrote a “k” after 4.7 above which means “kilo”, in its turn meaning “thousands”. This means that the resistor’s value is 4700 Ohms. You can think of it as the difference between 1 gram and 1 kilogram, but for resistor values!  

**Note:** If you buy the cheaper kit with 600 pieces, you won't get the exact values for this tutorial, which means you must add the next available value. This means: for example – instead of using 68 ohms which can be found in the 1460-pack, you would need to substitute it with a resistor value of 100 ohms from the 600-pack, which is the closest match (remember to use the next HIGHER value, because otherwise, you risk destroying your LEDs).  
**Price:** 199.90 SEK  
https://www.kjell.com/se/produkter/el-verktyg/elektronik/komponentsatser/playknowlogy-motstandssortiment-1460-pack-p90435


3 x 5mm LEDs (I used green, yellow, and red colors)  
**Note:** LEDs can be bought separately (more expensive) or in bulk (cheaper price per unit). Below is the LED kit I used for this project.  
**What it is:** LEDs (Light Emitting Diode) are simple lights. For this project, they are used to visualize the status of processes on the Raspberry Pi Pico WH, e.g., if it is running, or doing comparisons between temperature measurements.  
**Price:** 69.90 SEK  
https://www.kjell.com/se/produkter/el-verktyg/elektronik/komponentsatser/luxorparts-led-sortiment-5-mm-100-pack-p90418

1 x DS18B20 Waterproof temperature sensor  
**What it is:** It is a sensor that reads temperatures with high accuracy. Its range is -55°C to +85°C (±0.5°C). It is also waterproof which makes it perfect for this project since it is used to measure outdoor temperatures.  
**Price:** 195.00 SEK  
https://www.electrokit.com/produkt/temperatursensor-vattentat-ds18b20/

1 x DHT11 Temperature & humidity sensor  
**What it is:** It is a sensor that can measure both the temperature and the humidity. For this project, it's only used to measure temperatures.  
**Price:** 49.00 SEK  
https://www.electrokit.com/produkt/digital-temperatur-och-fuktsensor-dht11/




## Software
**Note:** This guide is written towards Windows-users, so you may have to adapt some instructions if you’re using for example MacOS or Linux as your operating system.  

### For this project, the following software is required (my version numbers in parentheses)  
- Thonny IDE: For writing code and running it on the Pico. (v4.0.2)
- Visual Studio Code: An Integrated Development Environment (IDE) suitable for creating and editing server projects. (v1.79.2)
- NodeJS: It's a server environment that in this case runs Node-Red. (v18.16.1)
- npm: It’s a tool to install packages/modules. (v9.5.1)
- MongoDB: The database that stores the recorded data. (v6.0.7)  

**Note:** The reason why I chose Thonny for writing code to the Raspberry Pi Pico WH is because it’s an “battery included” solution, which means it more or less works out of the box without any configurations (except for installing the firmware and choosing the correct COM-port for the Pico). Visual Studio Code needs additional packages and configurations to work, which adds further complexity to the project. For the sake of simplicity in this tutorial, we’ll use Thonny.  

Additionally, some npm packages are required. Essentially, npm packages are “modules”, or small applications that you can import/install into your project using the terminal in Visual Studio Code. In other programming languages the term “library” might also be used for the same concept.  

### npm packages to install (my version numbers in parentheses)  
- node-red: An environment where we can configure _flows_. (v3.0.2)
- node-red-dashboard: An additional module for visualizing the data. (v3.5.0) 
- node-red-node-mongodb: An additional module for easily integrating MongoDB into the _flows_. (v0.2.5)  

### Installing the npm packages
1. Create a new folder on your computer.
2. Open Visual Studio Code and browse to this folder to make it your working directory.
3. Run the following commands, one by one in the terminal of Visual Studio Code:  
- npm i node-red
- npm i node-red-dashboard
- npm i node-red-node-mongodb  

After each command, you will see a lot of text which you can ignore, unless it says something along the lines of “Cannot find module”, then you’ve written something wrong. Try again (make sure to write it exactly as above, or copy-paste it into the terminal).  

### Running Node-Red  
In the terminal of Visual Studio Code, enter the following command:  
- node node_modules/node-red/red.js  

You can stop the program at any time by pressing the buttons **CTRL** and **C** at the same time while having the terminal in focus.



## Firmware installation (installing MicroPython on the Raspberry Pi Pico WH)
1. Go to the following webpage: https://micropython.org/download/rp2-pico-w/
2. Click on the link that says something along the lines of:  
    - _v1.20.0 (2023-04-26) .uf2 [Release notes] (latest)_
    - **Note:** I use that exact firmware version as written above. The file I flashed the bootloader with is called:  
        _rp2-pico-w-20230426-v1.20.0.uf2_
3. Save this file to your PC.
4. On your Raspberry Pi Pico WH, there is a button next to the micro-USB port. This is called “BOOTSEL”. Press and hold this button while connecting your Pico to the PC.
5. A new storage media will appear in “My Computer”. It should be called: **RPI-RP2**.
6. Navigate into that folder and drag-and-drop the file you just downloaded into the folder.
7. Your pico might blink, and you might also hear the Windows USB connect/disconnect sounds a couple of times.
8. When the process is done, the storage media called: **RPI-RP2** will disappear from “My Computer”.
9. Done!  

**Note:** If you don't see the storage media **RPI-RP2** anymore, then don't worry. We will use the program Thonny to access and edit the contents of the Pico. We can still communicate with the Pico even if it is not visible on "My Computer".  


## Project assembly


### Schematics


#### Server (LCD)
![Display_bb](https://github.com/UtvactCoda/IoT-Project/assets/117079256/475e23f5-fa48-4c07-b5e0-fbc297710937)

#### Temperature sensor – Indoor (DHT11)  
**IMPORTANT! The DHT11 sensor exists in different variations. I am NOT using this specific version.
The sensor I'm using is right below this one (KY-015 version). I made this variant as an exercise in reading 
datasheets and creating diagrams, but also to possibly help those with the alternative 4-pin version 
as opposed to the 3-pin KY-015 version below. The wiring COULD possibly be faulty and have NOT been tested in a live circuit. Use at your own risk!**
![Indoor sensor_bb](https://github.com/UtvactCoda/IoT-Project/assets/117079256/9d3db386-f272-487c-9add-8287c4a992e1)


#### Temperature sensor – Indoor (DHT11, KY-015 version)  
**This is the version I'm using for this project.**
![Indoor sensor - KY-015_bb](https://github.com/UtvactCoda/IoT-Project/assets/117079256/4227d51e-b12c-4884-808d-9fda1fede85e)


#### Temperature sensor – Outdoor  
![Outdoor sensor_bb](https://github.com/UtvactCoda/IoT-Project/assets/117079256/371fdcb5-b78b-4315-98d1-f51d3dcce48a)


### Images of the project parts



## Uploading the code to your Raspberry Pi Pico WH  
The code to upload to the Picos are available here on GitHub. The files you need to download (or copy and paste the contents of) are specified below.  

### File structure  
- _./Code/_
    - _./Pico-WH/Common/credentials.py_
    - _./Pico-WH/Common/wifi.py_
    - _./Pico-WH/Sensors/Temperature/outdoor.py_
    - _./Pico-WH/Sensors/Temperature/indoor.py_
    - _./Pico-WH/Servers/lcd.py_

The “/” describes paths, or directories if you will; which describes where to find the code. There’s a folder called _“Code”_ here in the GitHub repository (where you are reading this tutorial) that you can look inside to find the code for all of the devices. The dot means "the current directory".  

The _/Common/_ folder contains code that is copied and run on all of the devices, regardless of their purpose (measuring values, serving web pages etc.). In this case they share the same network configuration as they are all using the same Wi-Fi network.

Using Thonny, upload the necessary _.py_-files for each device into their root folder (the top-most folder in the directory hierarchy). Below I have specified exactly which files should be uploaded to which Raspberry Pi Pico WH device.  

#### Pico WH - Server (The one with the LCD attached to it)
- _./Code/_
    - _./Pico-WH/Common/credentials.py_
    - _./Pico-WH/Common/wifi.py_
    - _./Pico-WH/Servers/lcd.py_

#### Pico WH - Outdoor (The one with the DS18B20 sensor attached to it)
- _./Code/_
    - _./Pico-WH/Common/credentials.py_
    - _./Pico-WH/Common/wifi.py_
    - _./Pico-WH/Sensors/Temperature/outdoor.py_

#### Pico WH - Indoor (The one with the DHT11 sensor attached to it)
- _./Code/_
    - _./Pico-WH/Common/credentials.py_
    - _./Pico-WH/Common/wifi.py_
    - _./Pico-WH/Sensors/Temperature/indoor.py_  
 

## How to run the code  
**Before running the code, ensure the following:**  
Your Raspberry Pi Pico WH must be selected as the code interpreter in the bottom right of Thonny. If not, then click on the text (it might say something along the lines of "_Local Python 3_"), and then select your Pico in the drop-down list. The text should look like:  
_MicroPython (Raspberry Pi Pico) COM **x**_  
The "**x**" is a number that corresponds to the port in which the Pico is connected to your PC.  

For each of the Picos, you should run the _.py_ file that corresponds to its role/name. For example, on the Pico WH - Server (with the LCD on it), run _lcd.py_, and for the Pico WH - Outdoor, run the _outdoor.py_ file, etc. You run the code by clicking on the green "Run" (play)-button in Thonny. To stop the program at any time, click the red "Stop"-button.



## Setting up Node-Red Flows  
A _flow_ describes a “chain reaction” of events, that happens sequentially one after another. These chain reactions can also happen I parallel, which make very complex systems possible in Node-Red. This makes it an excellent tool for this kind of project, since I want multiple things to happen at the same time – and when different things occur, I want different actions to happen depending on the type of events!  

**Note:** Because of a constraint on how many characters this tutorial can contain, I will therefore just summarize what happens in each flow, as opposed to going through the exact configurations. Therefore, you'll need to consult with the Node-Red documentation to find out how to set up the following flows (sorry for the inconvenience!).

### Flow: Incoming HTTP GET to MongoDB query: Fetch all sensor data - for the Server (LCD)  
![DB Sensors GET](https://github.com/UtvactCoda/IoT-Project/assets/117079256/28b7178b-005d-4256-b0a4-c5ff7c007c0c)

### Flow: Incoming Sensor HTTP POST to MongoDB insert  
![DB post sensor data](https://github.com/UtvactCoda/IoT-Project/assets/117079256/475edfd0-0040-40fb-8846-d0d7dcf3b2b4)

### Flow: Incoming Sensor HTTP POST to outgoing data to both Dashboard and Server (LCD)  
![LCD and Dashboard](https://github.com/UtvactCoda/IoT-Project/assets/117079256/36a6213a-352c-4ceb-9667-262dab128fc7)

### Flow: Compare sensor readings to find the lowest temperature and find out which sensor it belongs to (and store the result in a _flow_-variable)  
![Flow 1 and 2 with comparison](https://github.com/UtvactCoda/IoT-Project/assets/117079256/4b05d467-3abd-482e-a759-095231cf38c9)

### Flow (more complex): Check if the current sensor with the lowest reading is NOT the same as the last (the _flow_-variable), and if the condition is fulfilled, trigger a Switch that sends the new sensor to the Dashboard and the Server (LCD). A message is also embedded with a suggested action (close or open the windows, based on the previous comparison's value)  
![Flow 3](https://github.com/UtvactCoda/IoT-Project/assets/117079256/2d3f643c-09d9-4b4b-b73b-b8267a9b4082)


## Power consumption considerations
- Because the LCD display needs relatively much power, it is permanently connected to a wall socket power supply.
  - The energy consumption of the LCD depends on multiple factors, which includes but is not limited to: backlight brightness, refresh rate, pixel data and background processes, such as accepting GET requests and processing them, but also to update the screen in various ways (displaying data and showing text).
- The indoor temperature sensor uses additional LEDs besides performing the temperature measurements, which makes it draw more power — which is why it also uses a wall socket power supply.
- However, the indoor sensor ONLY measures temperatures/humidity (no LEDs or anything else) and reads the data every DELAY seconds (as defined in config.py). The power consumption is therefore determined by the constant's value (in essence, you the user decides this), but having it read the sensor value every 15 minutes should make it last for at least a couple of days (depending on how you power it). It would be suitable to use a powerbank with a bigger capacity, such as 2500 mAh.  
**Note:** if using a powerbank, ensure that it allows low power consumption (at least down to 20 mA), because otherwise it will automatically shut down, which then sets the Pico to be permanently off because it won't power up again once it loses power. A reconnection then has to be made to the powerbank (unplugging and plugging in the cable again).
- One could utilize deep sleep to further expand the battery life, which make the Picos go into "hibernation mode" between the measurements, but I had no time to implement this functionality. This could be an exercise for the reader.
- LoraWAN is very lightweight regarding power consumption, but the sensors ran out of stock, which is why I didn't use it in this project.
- Bluetooth Low Energy (BLE) is also "cheaper" in cost regarding battery consumption and for sending data, but as the Bluetooth functionality was recently released for the Pico W, there was not enough documentation in MicroPython for me to implement it. I therefore went with simple HTTP requests as the final choice as the communication protocol.
- Since the Wi-Fi is permanently on, it is NOT efficient to be used with batteries. See the point regarding deep sleep above.
- Using the maximum brightness of the LEDs shortens their lifespan. Therefore, a variable could be set in the config.py for brightness_level on a scale from 0 to 100, and then use Pulse Width Modulation (PWM) to control the brightness. (Not implemented; just an idea for further development of the software).

## Conclusion/Final thoughts  
I have been very happy with how this project turned out. In the beginning, the idea was to use a single sensor on a single pico, using Arduino Cloud to publish and visualize the data, but it felt too simple and I therefore wanted to challenge myself so I went with my own self-hosted "custom stack", inspired by the TIG-stack, but entirely switched out in functionality. The new stack is as follows (as a short summary):
- HTTP (communication protocol)
- Node-Red (server; flows coordination)
- MongoDB (storage)
- Pico with LCD display (local storage in a .txt file)
- Node-Red dashboard (visualization)
- Pico with LCD display (additional visualization)

### Additional information:
- Everything is run on a local network.
- The picos are connected to a separate guest network.
- The router is configured to block internet access on the picos (only "intranet" communication is allowed between the devices).
- I tried using MQTT with the Node-Red implementation of Aedes broker, but to no avail. There was too much troubleshooting. After several hours of trying without avail, I skipped MQTT entirely.
- If I could redo the project I'd change the Node-Red dashboard to Grafana, because it seems to offer a wider range of functionality and seems to work "out of the box" without complex configurations and additional software (custom nodes).
- My current project does not display historical data in the dashboard, this is because I didn't get the Node-Red chart node to work; only the Gauge and Text nodes, which renders saved data useless: I can only see what happens NOW. Unless I view the MongoDB data using MongoDBCompass.

## Credits
**The schematics and designs have been used for educational purposes.**  

The program used for creating the schematics is called Fritzing. Learn more about it here: https://fritzing.org/  

### Fritzing designs credits  
- Raspberry Pi: Pico W board:  
https://datasheets.raspberrypi.com/picow/PicoW-Fritzing.fzpz  

- Pico Display Pack 2.0 (pim580): vanepp:  
https://forum.fritzing.org/t/icp-10125-and-pim580/14644  

- DHT11: adafruit/Fritzing-Library GitHub repository:  
https://github.com/adafruit/Fritzing-Library  
License: https://github.com/adafruit/Fritzing-Library/blob/master/LICENSE.txt  
No changes were made to the original material.  

- DHT11 (KY-015): iot-lnu GitHub repository:  
https://github.com/iot-lnu/applied-iot/tree/master/Raspberry%20Pi%20Pico%20(W)%20Micropython/sensor-examples/P5_DHT_11_DHT_22/connection  
