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
ExecStart=/usr/bin/docker run --name bridge-service -p 4000:4000 jrgenerative/bebop-bridge-service-pi
ExecStop=/usr/bin/docker stop -t 5 bridge-service
ExecStopPost=/usr/bin/docker rm -f bridge-service

[Install]
WantedBy=default.target
```
### Bridge-Service Test Mode

Configure a script to run the bridge-service configured to use a Bebop mock-up implementation.

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

$>sudo systemctl daemon-reload
$>sudo systemctl start bebop-bridge-service.service
// try the app
$>sudo systemctl stop bebop-bridge-service.service



Test this with:
```
sudo systemctl daemon-reload
sudo systemctl start bebop-bridge-client.service
```
try the app
```
sudo systemctl stop bebop-bridge-client.service
```

To enable the service at system startup, execute:

sudo systemctl enable bebop-bridge-client.service




## Upgrade Docker image

Client 
$>sudo systemctl stop bebop-bridge-client.service
$> docker pull jrgenerative/bebop-bridge-client-pi
$>sudo systemctl start bebop-bridge-client.service

Service 
$>sudo systemctl stop bebop-bridge-service.service
$> docker pull jrgenerative/bebop-bridge-client-pi
$>sudo systemctl start bebop-bridge-service.service




Start and stop this sevice:

$>sudo systemctl start bebop-bridge-service-dummy.service
$>sudo systemctl stop bebop-bridge-service-dummy.service

