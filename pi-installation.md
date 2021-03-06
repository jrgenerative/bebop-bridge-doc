# RaspberryPi Installation

* Dowload the [Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) image you want to use. At the time of writing you can choose the default image which comes with the Pixel desktop, or the Lite version if you don't need a desktop.
* On Windows, use [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/) to flash the image onto the RaspberryPi microSD card. Make sure you select the correct drive before pressing the `write` button.
* Start your RaspberryPi and use nmap to discover its IP (on Windows you can use [Zenmap](https://nmap.org/zenmap/))`nmap -sn <ip-of-your-local-router>/24`, e.g. `nmap -sn 192.168.0.1/24`
* Login to your RaspberryPi via ssh with the Raspbian default user `pi` and default password `raspberry`. On Windows you can use [PuTTY](http://www.putty.org/).

# Enable SSH Server

On Raspbian Jessie with Pixel, the SSH server was somehow disabled by default. If you have a screen and a keyboard do:

```
sudo raspi-config
```

Go to `5 Interfacing Options` and enable SSH.
