```yaml
version: "3.9"
services:
  ddns-ipv64:
    image: alcapone1933/ddns-ipv64:latest
    mem_limit: 64M
    container_name: 04_DDNS-IPv64
    restart: always
    environment:
      - "TZ=Europe/Berlin"                  #   Please Change to your Time Zone.
      - "CRON_TIME=*/15 * * * *"
      - "CRON_TIME_DIG=*/30 * * * *"
      - "DOMAIN_IPV64=YOUR-DOMAIN.com"      #   Please Change to your Domain.
      - "DOMAIN_KEY=YOUR-PIN-CODE"          #   Please Change to your Domain-Key.
```