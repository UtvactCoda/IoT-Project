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

3 x 1k-Ohm resistor  
**Note:** I used a higher resistance value than necessary for the LEDs. This is because I wanted a more “dim” light, as the LEDs can sometimes be pretty intense with their (default) maximum brightness setting (although it is also possible to reduce the brightness of LEDs using Pulse Width Modulation (PWM)).
Resistors are often sold in bulk, which also makes it cheaper per unit. Below is a kit with a varied assortment of resistor values.  
**What it is:** It adds more resistance to a circuit, which gives components less power, i.e., LEDs get a lower brightness level in this case.  
**Price:** 159.90 SEK  
https://www.kjell.com/se/produkter/el-verktyg/elektronik/komponentsatser/playknowlogy-sortiment-med-resistorer-600-pack-p90646

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

**For this project, the following software is required**  
- Thonny IDE (for writing code and running it on the Pico)
- Visual Studio Code (for running Node-Red)
- NodeJS (it runs the server)
- npm (it’s a tool to install “packages”/modules in the NodeJS environment)
- Node-Red (it runs the “_flow_” system with parallel processes, on top of NodeJS)
- Additional Node-Red nodes (it is described in the section “_Setting up Node-Red Flows_” below)
- MongoDB (the database that stores the recorded data)

**Note:** The reason why I chose Thonny for writing code to the Raspberry Pi Pico WH is because it’s an “battery included” solution, which means it more or less works out of the box without any configurations (except for installing the firmware and choosing the correct COM-port). Visual Studio Code needs additional packages and configurations to work, which adds further complexity to the project. For the sake of simplicity in this tutorial, we’ll use Thonny.
