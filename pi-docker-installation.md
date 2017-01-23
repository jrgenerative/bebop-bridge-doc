# Docker Installation

## Prerequisites

* Raspbian Jessie is installed. The procedure described in what follows was tested with Raspbian 2017-01-11.

## Upgrade

Consider to upgrade your installation first via
```
sudo apt-get update
sudo apt-get dist-upgrade
```

## Install Docker

From [this](http://blog.alexellis.io/getting-started-with-docker-on-raspberry-pi/) documentation:

* `sudo curl -sSL get.docker.com | sh`
* `sudo usermod -aG docker pi`
* Start docker on boot `sudo systemctl enable docker`

Now reboot or start docker via
```
sudo systemctl start docker
```

