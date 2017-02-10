# Network Configuration

## Prerequisites & Assumptions

* [Raspbian Jessie 2017-01-11](https://www.raspberrypi.org/downloads/raspbian/) is installed on your Pi.
* 2 wlan interfaces named 'wlan0' and 'wlan1' show up on `ifconfig -a`.
* A network with SSID `<ssid-local>` is in range with a router at `192.168.0.1` and an address space `192.168.0.*`.
* Your Bebop 2 is switched on and has IP `192.168.42.1`.

## Approach

* Use `wlan0` to connect to an access point providing internet access.
* Use `wlan1` to connect to Bebop's access point at `192.168.42.1`.
* Configure a routing table such that requests to `192.168.42.*` are routed via `wlan`, everything else via `wlan0`.

## Basic Wifi Setup

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

Configure authentication to your local network with SSID `<ssid-local>` and pre-shared key `<psk-local>` for `wlan0`
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
Add the following
```
network={
 ssid="<ssid-local>"
 psk="<psk-local>"
}
```
Other lines such as
```
ctrl_interface=/var/....
update_config=1
```
can be left untouched.

Configure authentication to the Bebop network with SSID <ssid-bebop>` and pre-shared key `<psk-bebop>` for `wlan1`
```
sudo nano /etc wpa_supplicant/wpa_supplicant_bebop.conf
```
Add the following
```
network{
 ssid="<ssid-bebop>"
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

You should now see that `wlan0` joined `<ssid-local>` with IP `192.168.0.<xx>` and `wlan1` joined `<ssid-bebop>` with an IP assigned via dhcp from `192.168.42.*`.

## Routing between Bebop's interface (wlan1) and the interface to outer world (wlan0)

No additional configuration is required to be able to run the bebop-bridge with the above setup.

## Configuring to connect with wlan0 to different networks

If you want to configure wpa_supplicant to connect to different wireless networks depending on which is available, do the following:

In /etc/wpa_supplicant/wpa_supplicant.conf you can add multiple network configurations, but you must name them using the id_str attribute.
```
network={
    ssid="<ssid-01>"
    psk="<psk-01>"
    id_str="<any-name-01>"
}
network={
    ssid="<ssid-02>"
    psk="<psk-02>"
    id_str="<any-name-02>"
}
```

In /etc/network/interfaces set up your wlan0 configuration for example as follows (leave any other sections as is, e.g. for eth0):

```
allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf

iface <any-name-01> inet static
address <address-01>
gateway <gateway-01>
netmask <255.255.255.0>

iface home inet static
address <address-02>
gateway <gateway-02>
netmask <255.255.255.0>
```

