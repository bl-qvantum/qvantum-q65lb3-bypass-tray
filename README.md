# Blinky Bus Tray
## Table of contents
* Overview
* [Preparing the Raspberry Pi](#preparing-the-raspberry-pi)
* [Pair the Cube](#pair-th-cube)
* [Install the Tray](#install-the-tray)

## Overview
Blinky-Bus is a demonstration project on how to use Blinky-Lite with serial Bluetooth to communicate between the cube and tray. The function of the device is to turn on and off three LEDs. The Blinky-Lite tray software is written as a [Node-RED](https://nodered.org/) flow and can easily run on a Raspberry Pi.

You can obtain the source code for the cube by either cloning the repository or downloading a zip file from the green Code button on the [Github page](https://github.com/Blinky-Lite-Exchange/blinky-bus-tray).
<img src="doc/blinkyBusCube.jpg"/><br>

## Preparing the Raspberry Pi
Setup an Raspberry Pi with the latest [32 bit Raspberry Pi OS Lite](https://www.raspberrypi.com/software/operating-systems/). It is recommended to use the Raspberry Pi [Imager](https://www.raspberrypi.com/software/). Once the Raspberry Pi is up and running, SSH into the Raspberry Pi and download version 16.15.0 of [Node.js](https://nodejs.org/en/download/)

For ARM7 (Raspberry Pi 3)

    wget https://nodejs.org/dist/v16.15.0/node-v16.15.0-linux-armv7l.tar.xz

For ARM6 (Raspberry Pi Zero)

        wget https://unofficial-builds.nodejs.org/download/release/v16.15.0/node-v16.15.0-linux-armv6l.tar.xz

Extract the file

    tar -xf node-v16.15.0-linux-armvXl.tar.xz

where X is either 6 or 7.<br>Install the directory

    cd node-v16.15.0-linux-armvXl
    sudo cp -R * /usr/

where X is either 6 or 7.<br>Check to see the version

    node -v

You should see v16.15.0<br>
Remove the installation directory

    cd ..
    rm -rf node-v16.15.0-linux-armvXl
    rm node-v16.15.0-linux-armvXl.tar.xz

where X is either 6 or 7.

PM2 is a handy service to run programs in background and on boot. Next install PM2 globally

    sudo npm install -g pm2

## Pair the Cube
Next pair the Bluetooth of the Raspberry Pi with the Bluetooth of the cube. From the Raspberry Pi:

    sudo bluetoothctl

Once inside the bluetoothctl program enter:

    scan on

A list of available bluetooth devices will start to be listed. Once you see the name of your HC06 device that was setup in the [Blinky Bus Cube](https://github.com/Blinky-Lite-Exchange/blinky-bus-cube), turn the scan off:

    scan off

and pair the HC06 device

    pair XX:XX:XX:XX:XX:XX

where *XX:XX:XX:XX:XX:XX* is the MAC address of the HC06 device you saw on the scan list. The bluetoothctl program will ask you for the PIN number you setup in the [Blinky Bus Cube](https://github.com/Blinky-Lite-Exchange/blinky-bus-cube). After you have successfully paired the HC06 device, exit the bluetoothctl program:

    exit

Now you need to bind which Serial comm device for your HC06 device. At the Raspberry Pi terminal, enter:

    sudo rfcomm bind 0 XX:XX:XX:XX:XX:XX

where *XX:XX:XX:XX:XX:XX* is the MAC address of the HC06 device. This will bind the device serial device **/dev/rfcomm0** to the list of serial devices. You can see this device now by typing:

    ls /dev

To remember this binding, edit the rc.local file:

    sudo nano /etc/rc.local

and add the line  **sudo rfcomm bind 0** ***XX:XX:XX:XX:XX:XX*** right before the **exit 0** line in the file. To exit the nano editor type ***ctrl*** **x**

## Install the Tray
From the Raspberry Pi terminal, clone the tray repository or download the zip file from the green Code button on the [Github page](https://github.com/Blinky-Lite-Exchange/blinky-bus-tray).

Change directory into the tray directory:

    cd blinky-bus-tray

Install the code

    npm install

### Setup the environmental file
The **.env** file contains environmental variables that should not be shared. A template file **env** is stored on the Github repository Copy the env file to .env file

    cp env .env
https://nodered.org/
The **.env** file should look like:

    MQTTSUBSCRIBE=blinky-lite-v4/blinky-bus/01/setting/#
    MQTTCLIENTID=blinky-bus-tray-01
    MQTTSERVERIP=aaaa
    MQTTUSERNAME=bbb
    MQTTPASSWORD=ccc
    SERIALPORT=/dev/rfcomm0
    SERIALBUFSIZE=8
    PM2NAME=blinky-bus-01
    NODEREDCONFIGSECRET=dddd

You need to change the fields **aaaa**, **bbbb**, **cccc** to the appropriate values for the Blinky-Lite application Box you are going to connect to. You also need to pick a unique secret for the **dddd** field. This secret is used to [encrypt the MQTT credentials in Node-RED](https://discourse.nodered.org/t/your-flow-credentials-file-is-encrypted-using-a-system-generated-key/18382).

### Running Node RED
Before you run Node-RED, you need to change the adminAuth field in the settings.js file so you will be able to view and edit the tray flow.
