Eight Channel LoRaWAN Edge-Gateway
==============================
```
[ A Eight channel LoRaWAN multi-services and multi-optional gateway for communities ]
```
This repository contains a proof-of-concept implementation of eight chan lorawan edge gateway

It has been tested on the Raspberry Pi platform, using a Semtech SX1301 concentrator.

The code is for testing and development purposes only, and is not meant for production usage.

Maintainer: Pape Abdoulaye BARRO  <pape.abdoulaye.barro@gmail.com>

inspired on:
------------

```
- https://github.com/Lora-net/lora_gateway
- https://github.com/Lora-net/packet_forwarder
- https://www.loraserver.io/guides/debian-ubuntu/
```

Added new Features:
------------------

on `Makefile`:
	
```
- added graphic command;
- added mod command;
- added fireup command;
```
Added repositories:

```
- html repository for graphical setting
- modules repository ( loraserver, TIG and access point ) for Isolated network
- start.sh script to install and execute the dependencies specified in the Mainfile and then lauches the Packet Forwarder if all executions are successful. It also allows to upgrade the firmware as needed.
- edge-gateway.service to start the start.sh script.
```
Hardware Connection:
-------------------

```
In order to establish a SPI connection between the iC880A-SPI and the host system, the
following pins have to be used as minimum wiring set:


```
