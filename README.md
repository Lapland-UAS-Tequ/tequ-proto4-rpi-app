# tequ-proto4-rpi-app
 Raspberry PI application to read and process data from Movella Xsens acceleration&GNSS sensor unit. 
 - Data is streamed to Web dashboard (proto.tequ.fi) via Websocket
 - Logging to file can be enabled from UI
 - Data send to external MQTT broker can be enabled from UI
 - Logging and data sending is only done if GNSS status is valid

This repository is developed and tested using Raspberry PI 5 8 GB. Software and components should be compatible with older Raspberry PI boards, but not tested.

Node-RED editor address

- http://localhost:1880 / http://RPI_IP:1880

Node-RED dashboard address:

- http://localhost:1880/dashboard / http://RPI_IP:1880/dashboard 

## Hardware components

| Hardware                  | Product number       | Website |
| -------------             |:-------------:| :-------------:|
| Rasbperry PI5    | PI5-8GB    | [link](https://www.raspberrypi.com/products/raspberry-pi-5/)|
| Pimoroni NVMe base         | PIM699  | [link](https://shop.pimoroni.com/products/nvme-base)|
| NVMe memory    | SKC3000S/512G  | [link](https://www.dustin.fi/product/5011261755/kc3000)|
| Sixfab 4G/LTE base HAT     | S121 | [link](https://sixfab.com/product/raspberry-pi-base-hat-3g-4g-lte-minipcie-cards/)|
| Quectel 4G module    | S60  | [link](https://sixfab.com/product/quectel-ec25-mini-pcie-4g-lte-module/)|
| Movella XSens sensor    | MTi-G-710 GNSS/INS | [link](https://www.movella.com/products/sensor-modules/xsens-mti-g-710-gnss-ins)|
| Movella XSens USB cable |  CA-USB-MTI | [link](https://shop.movella.com/product-lines/sensor-modules/accessories/ca-usb-mti)|
| Lenovo USB-C car power supply | 40AK0065WW  | [link](https://www.dustin.fi/product/5011112082/65w-usb-c-dc-travel-adapter)|

## Preparation of components

1. Build and connect NVMe base
   
   - https://learn.pimoroni.com/article/getting-started-with-nvme-base

2. Build and connect 4G/LTE base HAT
   
   - https://docs.sixfab.com/docs/getting-started-with-base-hat-and-quectel-ec25-eg25-module

## Configure 4G module

Detailed instructions:

- https://docs.sixfab.com/page/cellular-internet-connection-in-ecm-mode

Short instructions:

Remove modemmanager

```
sudo apt purge modemmanager -y
```

Find out modem serial port (send AT command using default serial settings 115200 8N1 to USB-ports)

Use terminal program or Node-RED or any other way to configure 4G-module with following commands

Configure APN
```
AT+CGDCONT=1,"IPV4V6","internet"
```

Configure USBnet mode
```
AT+QCFG="usbnet",1
```

Reboot module (might not needed)
```
AT+CFUN=1,1
```

## Configure Movella Xsens sensor

- Download and install MT Software Suite https://www.movella.com/support/software-documentation
- Connect sensor with USB cable
- Configure serial mode and baudrate to 2000000
- Enable NMEA sentences PSONCMS and GPRMC

## Map Movella Xsens sensor to specific USB port when connected to Raspberry PI

Create and edit file
```
sudo nano /etc/udev/rules.d/10-usb-serial.rules
```

Add following line to file:
```
SUBSYSTEM=="tty", ATTRS{idProduct}=="d00d", ATTRS{idVendor}=="2639", SYMLINK+="ttyUSB_XSENS"
```

## Configure wifi access point settings-file

```
sudo nano /home/pi/.node-red/wifi-settings.json
```

```
{"ssid":"your_ssid","password":"your_password!"}
```


## Raspberry PI software installation steps

```
sudo apt update && sudo apt upgrade
```

```
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

```
sudo systemctl enable nodered.service
```

```
cd ~/.node-red
```

```
npm install @flowfuse/node-red-dashboard &&
npm install node-red-contrib-nmea &&
npm install node-red-contrib-moment &&
npm install node-red-contrib-msg-speed &&
npm install node-red-contrib-web-worldmap &&
npm install node-red-node-serialport &&
```

## Install Node-RED flows
1. Copy flows.json from this repository to your node-red folder
2. Restart Node-RED
3. Configure MQTT node credentials
