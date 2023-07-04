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
