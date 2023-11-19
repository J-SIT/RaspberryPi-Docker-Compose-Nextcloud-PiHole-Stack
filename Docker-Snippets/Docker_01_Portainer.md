```sh
docker run -d -p 8000:8000 -p 9443:9443 --name 01_Portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/01_Portainer:/data portainer/portainer-ce:latest
```