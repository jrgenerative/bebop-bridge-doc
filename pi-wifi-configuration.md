# Network Configuration

## Prerequisites & Assumptions

* [Raspbian Jessie 2017-01-11](https://www.raspberrypi.org/downloads/raspbian/) is installed on your Pi.
* 2 wlan interfaces named 'wlan0' and 'wlan1'.
* With `wlan0` the network `192.168.0.*` is joined with a router at `192.168.0.1`.
* Bebop has IP `192.168.42.1`.

## Approach

* Use `wlan0` to connect to an access point providing internet access.
* Use `wlan1` to connect to Bebop's access point at `192.168.42.1`.
* Configure a routing table such that requests to `192.168.42.*` are routed via `wlan`, everything else via `wlan0`.

## Wifi Setup

Edit your network configuration via
```
sudo nano /etc/network/interfaces
```

To connect `wlan0` to your local network (with internet access) edit or add the `wlan0` section as
```
allow-hotplug wlan0
 iface wlan0 inet static
 address 192.168.0.<xx>
 netmask 255.255.255.0
 gateway 192.168.0.1
 wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

To connect with `wlan1` to Bebop, edit or add the `wlan1` section as
```
allow-hotplug wlan1
 iface wlan1 inet manual
 wpa-conf /etc/wpa_supplicant/wpa_supplicant_bebop.conf
```

Configure authentication to your local network with SSID `<ssid>` and pre-shared key `<psk>` for `wlan0`
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
Add the following
```
network={
 ssid="<ssid>"
 psk="<psk>"
}
```
Other lines such as
```
ctrl_interface=/var/....
update_config=1
```
can be left untouched.

Configure authentication to the Bebop network with SSID <Bebop2-xxxxxx>` and pre-shared key `<psk-bebop>` for `wlan1`
```
sudo nano /etc wpa_supplicant/wpa_supplicant_bebop.conf
```
Add the following
```
network{
 ssid="<Bebop2-xxxxxx>"
 psk="<psk-bebop>"
 key_mgmt=NONE
}
```
Unfortunately, Bebop 2 has by default an open network which is why `key_mgmt=NONE` is required.

Test your configuration with
```
sudo ifdown wlan0 && sudo ifup wlan0
sudo ifdown wlan1 && sudo ifup wlan1
ifconfig
```
## Routing

[TODO]
