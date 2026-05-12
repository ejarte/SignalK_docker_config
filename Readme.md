# Xthyra signalk docker config
Followed this guide https://seabits.com/set-up-signal-k-and-grafana-on-raspberry-pi-with-pican-m-nmea-2000-board/

https://manuals.plus/sk-pang/rsp-pican-m-case-metal-case-for-pican-m-manual


# Hardware
- Raspberry Pi 4 or similar
- PICAN-M


# Prerequisites

## Install Pi OS
- Download the latest Raspberry Pi OS Lite from https://www.raspberrypi.com/software/operating-systems/

Flash the image to a microSD card using Raspberry Pi Imager.

## Install Docker
https://docs.docker.com/engine/install/debian/

https://docs.docker.com/engine/install/linux-postinstall/


# Cloudflared
Sign in on the Cloudflare dashboard and create a new team. Then create a new tunnel and get the CLOUDFLARE_TUNNEL_TOKEN.

https://hub.docker.com/r/cloudflare/cloudflared

https://dash.teams.cloudflare.com/



# Usage
- Clone the repository

## Can0
First do this to activare SPI!
```
sudo raspi-config
Select Interfacing Options -> SPI -> Yes to enable SPI interface
sudo reboot
```


```bash
sudo vim /boot/firmware/config.txt
```

add the following lines to the end of the file:
```
[PICAN-M]
enable_uart=1
dtparam=i2c_arm=on
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=16000000,interrupt=25
dtoverlay=spi-bcm2835-overlay
```

```
[2-CH_CAN_HAT+]
dtparam=spi=on
dtoverlay=i2c0 
dtoverlay=spi1-3cs
dtoverlay=mcp2515,spi1-1,oscillator=16000000,interrupt=22
dtoverlay=mcp2515,spi1-2,oscillator=16000000,interrupt=13

```



```bash
sudo apt install can-utils -y
```

```bash
sudo /sbin/ip link set can0 up type can bitrate 250000
```

```bash
vim socketcan-interface.service
````

```service
[Unit]
Description=SocketCAN interface can0 with a baudrate of 250000
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/sbin/ip link set can0 type can bitrate 250000 ; /sbin/ifconfig can0 up
ExecReload=/sbin/ifconfig can0 down ; /sbin/ip link set can0 type can bitrate 250000 ; /sbin/ifconfig can0 up
ExecStop=/sbin/ifconfig can0 down
[Install]
WantedBy=multi-user.target
```

```bash
sudo cp socketcan-interface.service /etc/systemd/system
sudo chmod 644 /etc/systemd/system/socketcan-interface.service
sudo systemctl enable socketcan-interface.service
```


## Copy files
```bash
scp -i ~/.ssh/id_rsa -rp ../XthyrasignalkDockerConfig pi@xthyra.local:
```

## Influx DB
To configure InfluxDB tokens you can use the Influx UI with help of the following command to access it via SSH tunnel:

```bash
ssh -L 8086:localhost:8086  pi@sshxthyra.storanassa.se
```

## Grafana


## Signalk

To configure SignalK, you can use the SignalK UI with help of the following command to access it via SSH tunnel:

```bash
ssh -L 3000:localhost:3000 pi@xthyra.local
```

### Signalk add can0


