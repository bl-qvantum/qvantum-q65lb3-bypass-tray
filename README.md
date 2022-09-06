# Blinky Bus Tray
Blinky-Bus is a demonstration project on how to use Blinky-Lite with serial Bluetooth to communicate between the cube and tray. The function of the device is to turn on and off three LEDs.
## Preparing the Tray
Setup an Raspberry Pi with the latest [32 bit Raspberry Pi OS Lite](https://www.raspberrypi.com/software/operating-systems/). It is recommended to use the Raspberry Pi [Imager](https://www.raspberrypi.com/software/). Once the Raspberry Pi is up and running, from the Raspberry Pi download version 16.15.0 of [Node.js](https://nodejs.org/en/download/)

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
