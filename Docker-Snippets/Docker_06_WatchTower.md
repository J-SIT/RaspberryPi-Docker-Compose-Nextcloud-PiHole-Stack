```yaml
version: "3"
services:
  watchtower:
    container_name: 06_WatchTower
    mem_limit: 128M
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    restart: always
```