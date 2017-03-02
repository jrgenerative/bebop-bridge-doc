# Installing the Bebop-Bridge

## Download the Application

```
docker pull jrgenerative/bebop-bridge-client-pi
docker pull jrgenerative/bebop-bridge-service-pi
```
## Add Start-Up Scripts

### Bridge-Client 

In `/etc/systemd/system` add a file, e.g. `bebop-bridge-client.service` with content:
```
[Unit]
Description=Bebop Bridge Client in a Docker container
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker run --name bridge-client -p 8080:8080 jrgenerative/bebop-bridge-client-pi
ExecStop=/usr/bin/docker stop -t 10 bridge-client
ExecStopPost=/usr/bin/docker rm -f bridge-client

[Install]
WantedBy=default.target
```

### Bridge-Service

In `/etc/systemd/system` add a file, e.g. `bebop-bridge-client.service` with content:

```
[Unit]
Description=Bebop Bridge Service in a Docker container
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker run --name bridge-service -p 4000:4000 -p 43210:43210/udp -p 55636:55636/udp -p 44820:44820/udp -p 54321:54321/udp -p 44444:44444/udp jrgenerative/bebop-bridge-service-pi
ExecStop=/usr/bin/docker stop -t 5 bridge-service
ExecStopPost=/usr/bin/docker rm -f bridge-service

[Install]
WantedBy=default.target
```
### Bridge-Service Test Mode

Configure a script to run the bridge-service with a mock-up implementation of Bebop.

In `/etc/systemd/system` add a file, e.g. `bebop-bridge-service-dummy.service` with content:

```
[Unit]
Description=Bebop Bridge Service in a Docker container
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker run --name bridge-service-dummy -p 4000:4000 -e "DRONE=dummy" jrgenerative/bebop-bridge-service-pi
ExecStop=/usr/bin/docker stop -t 5 bridge-service-dummy
ExecStopPost=/usr/bin/docker rm -f bridge-service-dummy

[Install]
WantedBy=default.target
```

## Test the Installation

After putting in place above scripts
```
sudo systemctl daemon-reload
```

To start the application
```
sudo systemctl start bebop-bridge-service.service
sudo systemctl start bebop-bridge-client.service
```

Point your browser to the address of your server, port `8080`.

To stop the application
```
sudo systemctl stop bebop-bridge-client.service
sudo systemctl stop bebop-bridge-service.service
```

## Start the Application at System Startup

To enable starting the bebop-bridge application automatically at system start, execute:
```
sudo systemctl enable bebop-bridge-client.service
sudo systemctl enable bebop-bridge-service.service
```

## Upgrade the Application
 
```
#!/bin/bash

set -x

echo "Stopping bebop-bridge services ..."
sudo systemctl stop bebop-bridge-client.service
sudo systemctl stop bebop-bridge-service.service

echo "Pulling latest images ..."
docker pull jrgenerative/bebop-bridge-client-pi
docker pull jrgenerative/bebop-bridge-client-pi

echo "Starting bebop-bridge services ..."
sudo systemctl start bebop-bridge-client.service
sudo systemctl start bebop-bridge-service.service

# Remove exited and untagged images

set -o errexit

echo "Removing exited docker containers..."
docker ps -a -f status=exited -q | xargs -r docker rm -v

echo "Removing untagged images..."
docker images --no-trunc | grep "<none>" | awk '{print $3}' | xargs -r docker rmi

echo "Done"

read -p "Press any key to continue ..."
```
