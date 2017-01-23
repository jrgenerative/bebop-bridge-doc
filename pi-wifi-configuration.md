# Network Configuration

## Prerequisites

* Installed [Raspbian Jessie 2017-01-11](https://www.raspberrypi.org/downloads/raspbian/)
* 2 wlan interfaces named 'wlan0' and 'wlan1'

## Approach

* Use `wlan0` to connect to an access point providing internet access.
* Use `wlan1` to connect to Bebop's access point at `192.168.42.1`.
* Configure a routing table such that requests to `192.168.42.*` are routed via `wlan`, everything else via `wlan0`.

## Wifi Setup

## Routing

