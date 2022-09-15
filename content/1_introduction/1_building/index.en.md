---
title : "What are we Building"
weight : 10
---




## Hardware

![hardware](/static/building/full.jpg)

Each group has a setup similar to this.  In the middle is the base station, which has a group number on it.  This is the team you are on and also number you will use through the workshop.

On either side are two IoT watches with attached gas sensors.  They can be unplugged and worn like any other watch, but with a great deal more functionality.


### Watch
We will be building wearable devices, IoT watches, that will act as a connected gas warning device.  It will receive messages from AWS to warn the wearer about potential hazardous environments, while also reporting back it's own gas readings in real-time.  It is based off the M5StickC Plus (ESP32 microprocessor) with the M5 TVOC environmental sensor (SGP30).  The ESP32 is a low-power, but powerful microcontroller which also has built-in WiFi and Bluetooth communication abilities, perfect for portable devices.

![watch](/static/building/watchwear.jpg)

The watch will change colors based on the information received, either locally from it's own sensor or from AWS services that we will build out.  From the beginning, the watch has been programmed to react to it's own sensor.  However, in this workshop we will set things up so that it can also be affected by conditions of other watches and the base station.

### Watch Warning States:

**Green**:  All is good, gas reading is below the threshold.
**Yellow**:  Another watch in the group has exceeded their threshold.
**Orange**:  The base station has exceeded it's threshold.  
**Red**:  The watch itself has detected gasses that exceed the threshold.

![watchgreen](/static/building/watchgreen.jpg)
![watchyellow](/static/building/watchyellow.jpg).
![watchorange](/static/building/watchorange.jpg)
![watchred](/static/building/watchred.jpg)

### Base Station
We will also build out a base station.  This is an example of a device that is often present at facilities, either directly attached to hardware and monitoring all the sensors attached, or on the network as part of the Process Control Network (PCN) or Process Information Network (PIN) and consuming the sensor telemetry.  It is more powerful than a simple microcontroller and is capable of running a full operating system.  As such, we can run the AWS Greengrass software on it.  Here, we will be using a Raspberry Pi computer, running Linux, with an attached Bosch BME688 gas sensor.

![base](/static/building/base.jpg)

Just as with the watches, the base station will have a **green** and **red** state.  It will not be working at the start of the workshop, but we will work towards deploying the necessary components with AWS Greengrass.


All devices will be pre-provisioned in our accounts, so we will not need to worry about installing software or handling device certificates directly on the devices.


## Architecture

![arch](/static/building/arch.png)

Our journey has 4 main steps:
- Get data flowing between the devices and AWS.
- Setup the AWS services that will react to events and store the data.
- Route the data to the different AWS services as needed.
- Visualize!

## Teams

We will be working in teams to complete this workshop.  There are 10 teams, which is designated by the label on the base station.  Each team has 1 base station device and 2 watches, all mounted on a din rail.  Feel free to unplug the watches and try them on!  The battery life is about 1 hour, as the device code has not been optimized yet.

## Important

This workshop is made up of 3 phases of workflows. Each phase relies upon the previous phase being complete. The workflows in each phase can be completed in parallel.  As we are working in teams, this will give us the opportunity to work together in parallel to complete each phase faster.

The workshop is intended to be run in the US-East-1 region (N. Virginia).  
