```yaml
version: "3"
services:
  pihole:
    container_name: 05_Pi-Hole
    mem_limit: 512M
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "8480:80/tcp"
    hostname: raspberrypi
    environment:
      TZ: 'Europe/Berlin'
      WEBPASSWORD: 's74F444cqjL8pK5Wypy6sh8b39QYEo'         #   Please Change the PW.
      DNSSEC: 'true'
      FTL_CMD: 'no-daemon -- --dns-forward-max 600'
    volumes:
      - '/docker/05_PiHole/etc-pihole:/etc/pihole'
      - '/docker/05_PiHole/etc-dnsmasq.d:/etc/dnsmasq.d'
    restart: unless-stopped
```