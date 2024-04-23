# tequ-proto4-rpi-app
 Raspberry PI 5 application to read and process data from Xsens acceleration&GNSS sensor unit. 
 - Data is streamed to Web dashboard (proto.tequ.fi) via Websocket
 - Local logging to file can be enabled from UI
 - Data is sent to Tequ MQTT broker
 - Logging and data sending is only done if GNSS status is valid

Editor address: http://localhost:1880 / http://RPI_IP:1880

Dashboard address: http://localhost:1880/dashboard / http://RPI_IP:1880/dashboard 

## Hardware components

| Hardware                  | Product number       | Website |
| -------------             |:-------------:| :-------------:|
| Rasbperry PI5    | PI5-8GB    | [link](https://www.raspberrypi.com/products/raspberry-pi-5/)|
| NVMe base         | PIM699  | [link](https://shop.pimoroni.com/products/nvme-base)|
| NVMe memory    | SKC3000S/512G  | [link](https://www.dustin.fi/product/5011261755/kc3000)|
| 4G/LTE base HAT     | S121 | [link](https://sixfab.com/product/raspberry-pi-base-hat-3g-4g-lte-minipcie-cards/)|
| 4G module    | S60  | [link](https://sixfab.com/product/quectel-ec25-mini-pcie-4g-lte-module/)|
| XSens sensor    | MTi-G-710 GNSS/INS | [link](https://www.movella.com/products/sensor-modules/xsens-mti-g-710-gnss-ins)|
| XSens USB cable |  CA-USB-MTI | [link](https://shop.movella.com/product-lines/sensor-modules/accessories/ca-usb-mti)|
| USB-C car power supply | 40AK0065WW  | [link](https://www.dustin.fi/product/5011112082/65w-usb-c-dc-travel-adapter)|

## Preparation of components

1. Build and connect NVMe base https://learn.pimoroni.com/article/getting-started-with-nvme-base
2. Build and connect 4G/LTE base HAT https://docs.sixfab.com/docs/getting-started-with-base-hat-and-quectel-ec25-eg25-module

## Raspberry PI 5 software installation steps

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
3. Configure MQTT node credentials (user: proto4)
