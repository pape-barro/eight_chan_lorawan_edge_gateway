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

![1](https://github.com/pape-barro/eight_chan_lorawan_edge_gateway/blob/master/content/1.png)
```
The power pins (21, 22) have to be connected to a power source that is able to provide more than 700 mA.
```
![2](https://github.com/pape-barro/eight_chan_lorawan_edge_gateway/blob/master/content/2.png)
```
Therefore it is recommended to choose a proper power supply that is able to supply the iC880A-SPI and the host system (e.g. Raspberry PI) at the same time.
```
![3](https://github.com/pape-barro/eight_chan_lorawan_edge_gateway/blob/master/content/3.png)
```
It is recommended to connect the necessary signals using wires that are as short possible in order to prevent communication disorders.
```
Installation
------------
```
$ cd /opt/
$ sudo git clone https://github.com/pape-barro/edge-gateway.git ./edge-gateway
$ cd /opt/edge-gateway/
$ sudo make
$ sudo make install
$ sudo make graphic
$ sudo -u postgres psql
	> create role loraserver_as with login password 'dbpassword';
	> create role loraserver_ns with login password 'dbpassword';
	> create database loraserver_as with owner loraserver_as;
	> create database loraserver_ns with owner loraserver_ns;
	> \c loraserver_as
	> create extension pg_trgm;
	> \q
```
find the version of raspbian and choise:
```
$ echo "deb https://repos.influxdata.com/debian jessie stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
```
OR:
```
echo "deb https://repos.influxdata.com/debian stretch stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
```
next:
```
$ sudo make mod
$ influx
	> CREATE USER admin WITH PASSWORD 'dbpassword' WITH ALL PRIVILEGES
	> exit
$ influx -username admin -password dbpassword
	> CREATE DATABASE telegraf
	> exit
$ sudo make fireup

```
To find out which version of raspbian you’re running
-------------
```
$ cat /etc/os-release
```

To start service (should already be started at boot if you done make install and rebooted of course), stop service or look service status:
------------------------------------------------------------------------------------------------------------------------------------------
```
$ sudo systemctl start edge-gateway
$ sudo systemctl stop edge-gateway
$ sudo systemctl status edge-gateway
```
To see packet forwarder log in real time:
-------------------------------
```
$ sudo journalctl -f -u edge-gateway
```
To see LoRa-gateway-bridge log-output:
-------------------------------
```
$ sudo journalctl -u lora-gateway-bridge -f -n 50
```
To see LoRa Server log-output:
-------------------------------
```
$ sudo journalctl -f -n 100 -u loraserver
```
To see LoRa App Server log-output:
-------------------------------
```
$ sudo journalctl -f -n 100 -u lora-app-server
```

Dependencies
------------
```
- SPI needs to be enabled on the Raspberry Pi (use raspi-config)
- WiringPi: a GPIO access library written in C for the BCM2835
  used in the Raspberry Pi.
  sudo apt-get install wiringpi
  see http://wiringpi.com
- Run packet forwarder as root
```
