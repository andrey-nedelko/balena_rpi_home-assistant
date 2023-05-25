# home assistant + zigbee2mqtt + mosquitto + duplicati + hass-config + nginx on Balena
Easy to deploy smart home server powered by Home Assistant. Secure remote access out of the box. Automatic backups on Google Drive.

## Contents
List of containers installed on device:
- home assistant
- zigbee2mqtt
- mosquitto broker
- duplicati
- hass configurator
- nginx reverse proxy

## Hardware required
Here’s the list of items required for a basic setup:

* Raspberry Pi 3B or greater (A B+ or 4B works great, and less powerful Pis can be used albeit with lower performance)
* 8GB (or larger) Micro-SD Card
* Power supply and cable
* Zigbee coordinator

## Software required
This repository contains all of the software and configuration you’ll need to get started. We’re going to deploy this project on balenaCloud using a free account to push the project and all the software to your Raspberry Pi as well as to provide remote access. Therefore, you’ll need:

* A tool to flash your SD card, such as [balenaEtcher](https://www.balena.io/etcher/)
* A free [balenaCloud](https://dashboard.balena-cloud.com/login) account
* A clone or download of our project from GitHub

## Software setup

1. Clone this repo
2. Change `docker-compose.yaml` line `/dev/ttyUSB0:/dev/ttyACM0` to `/dev/ttyACM0:/dev/ttyACM0` if you need to change zigbee coordinator port 
3. Use the [balenaCLI](https://github.com/balena-io/balena-cli) to push to your devices. [Read more](https://www.balena.io/docs/learn/deploy/deployment/).

### First login
Browse to device's IP adress (You can find the device's IP address in your balenaCloud dashboard) on port 3218 (for example http://192.168.1.42:3218 or http://192.168.1.42/hass-conf/) to access hass-configurator.  Open `configuration.yaml` and add this lines to enable reverse proxy:
```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 127.0.0.0/24
    - 172.19.0.0/24
    - 192.168.1.0/24
    - ::1
  ip_ban_enabled: true
  login_attempts_threshold: 5
```
  After this go to balena device dashboard, restart homeassistant container and also obtain a secure public URL for your Home Assistant instance - simply click the "Public Device URL" switch on your balenaCloud dashboard. You'll then see a link to access your unique device URL.
  Setup is done. You can access your device and installed webapps via this addresses:
  - `https://PUBLIC_URL/` - home assistant
  - `https://PUBLIC_URL/hass-conf/` - hass configurator
  - `https://PUBLIC_URL/z2m` - zigbee2mqtt
  - `https://PUBLIC_URL/duplicati` - duplicati backups

  default basic auth: `admin admin`

