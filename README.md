# bebop-bridge-doc

Documentation for the bebop-bridge application

1. [Setting up your RaspberryPi](https://github.com/jrgenerative/bebop-bridge-doc/blob/master/pi-installation.md)
2. [Configure your network](https://github.com/jrgenerative/bebop-bridge-doc/blob/master/pi-wifi-configuration.md)
3. [Install Docker](https://github.com/jrgenerative/bebop-bridge-doc/blob/master/pi-docker-installation.md)
4. [Install and run Bebop-Bridge](https://github.com/jrgenerative/bebop-bridge-doc/blob/master/pi-bebop-bridge-installation.md)

# Debug network traffic between Bebop and service

To see back and forth traffice from port to port:

```
tcpdump -i wlan1 
```


## Send test package (once)

Send udp message to 192.168.0.199 43210

`echo -n “foo my message goes here” | nc -4u -w1 192.168.0.199 43210`

Send tcp message to 192.168.0.199 4000

`echo -n “foo my message goes here” | nc -4 -w1 192.168.0.199 4000`

## Listen to traffice in a running Docker container:

Need to install some stuff:
```
docker exec bridge-service apt-get update
docker exec bridge-service apt-get --assume-yes install netcat // for nc
docker exec bridge-service apt-get --assume-yes install net-tools // for ifconfig
or
docker exec bridge-service apt-get --assume-yes install tcpdump // for tcpdump
```

Listen to all traffic:

`docker exec bridge-service tcpdump -i eth0`

Listen to traffic via port 4000 (socket.io connection of controller)

`docker exec bridge-service tcpdump -i eth0 -n tcp dst port 4000`

Listen to udp traffice on 43210

`docker exec bridge-service tcpdump -i eth0 -n -vv udp dst port 43210`

## Listen on Linux to traffic

Listen to udp messages from Bebop when connected with wlan1 to Bebop network

`sudo tcpdump -i wlan1 -n udp dst port 43210`

## Listen on Windows with Wireshark 

Listen to messages from Bebop when connected with interface 1 to Bebop network. (Check with `wininfo32` -> Components -> Network for interface index.)

In Wireshark: use filter: `udp`, or filter: `udp port 43210`, or `tcp port 4000`



