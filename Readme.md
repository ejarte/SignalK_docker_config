# Xthyra signalk docker config
Followed this guide https://seabits.com/set-up-signal-k-and-grafana-on-raspberry-pi-with-pican-m-nmea-2000-board/

https://manuals.plus/sk-pang/rsp-pican-m-case-metal-case-for-pican-m-manual


# Hardware
- Raspberry Pi 4 or similar
- PICAN-M


# Prerequisites

## Install Pi OS
- Download the latest Raspberry Pi OS Lite from https://www.raspberrypi.com/software/operating-systems/


## Install Docker
https://docs.docker.com/engine/install/debian/

https://docs.docker.com/engine/install/linux-postinstall/


# Cloudflared
Sign in on the Cloudflare dashboard and create a new team. Then create a new tunnel and get the CLOUDFLARE_TUNNEL_TOKEN.

https://hub.docker.com/r/cloudflare/cloudflared

https://dash.teams.cloudflare.com/



# Usage
- Clone the repository


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
