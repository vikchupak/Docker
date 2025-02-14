See docker configuration
```bash
docker info
# "Insecure Registries"
```

```bash
/etc/docker/daemon.json
```

```bash
/var/run/docker.sock
```

```bash
~/.docker/config.json
```

```bash
sudo systemctl restart docker
```

# Install as snap

https://askubuntu.com/questions/941816/permission-denied-when-running-docker-after-installing-it-as-a-snap

```bash
sudo snap install docker
sudo groupadd docker && sudo usermod -aG docker $USER && newgrp docker
sudo snap disable docker
sudo snap enable docker
```

`docker.sock` location inside snap
```bash
docker context ls
# DOCKER ENDPOINT: unix:///var/run/docker.sock
```

Real `docker.sock` location on host
```bash
sudo find / -name docker.sock
# /run/docker.sock
```

![image](https://github.com/user-attachments/assets/95c6b47f-29c2-4e48-9a94-822886da4d4b)

![image](https://github.com/user-attachments/assets/1bbbb3cb-81ec-452c-9a44-34b439afeef5)
