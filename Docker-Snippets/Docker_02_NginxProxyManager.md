```yaml
version: '3'
services:
  npm:
    image: 'jc21/nginx-proxy-manager:latest'
    mem_limit: 128M
    container_name: 02_NginxProxyManager
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_SQLLITE_FILE: "/docker/02_NginxProxyManager/data/npm.sqlite"
    volumes:
      - /docker/02_NginxProxyManager/data:/data
      - /docker/02_NginxProxyManager/letsencrypt:/etc/letsencrypt
```