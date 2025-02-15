# Useful commands to debug docker installation

See docker configuration
```bash
docker info
# "Insecure Registries"
```

See `docker.sock` listening location
```bash
docker context ls
# DOCKER ENDPOINT: unix:///var/run/docker.sock
```

Find config files when unsure
```bash
sudo find / -name daemon.json
# /var/snap/docker/2976/config/daemon.json
```

# Using apt

Just follow official doc
- https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

## apt config files location

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

# Using snap

https://askubuntu.com/questions/941816/permission-denied-when-running-docker-after-installing-it-as-a-snap

compose is included
```bash
sudo snap install docker
sudo groupadd docker && sudo usermod -aG docker $USER && newgrp docker
sudo snap restart docker
```

## snap config files location

`docker.sock` listening location
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

```bash
sudo find / -name daemon.json
# /var/snap/docker/2976/config/daemon.json
```

# Using Docker Desktop

Just follow official doc for Docker-Desktop installation
- https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

## Docker Desktop config files location

Docker-desktop docker.sock location
```bash
But mounting via volume doesn't work for this sock
/home/viktor/.docker/desktop/docker.sock
```

```bash
/home/viktor/.docker/daemon.json
```

## Mount Docker Desktop sock

```bash
Change socket group from viktor to docker
sudo chown :docker /home/viktor/.docker/desktop/docker.sock
```

```bash
# Create symbolic to Docker Desktop socket from default docker.sock location
# This makes Docker Desktop "think" there is defaul docker context
# But actually it points to itself
sudo ln -s /home/viktor/.docker/desktop/docker.sock /var/run/docker.sock
```

```bash
# Inside container add the container user to docker group with the same GID as on host
groupadd -g 999 docker && usermod -aG docker jenkins
```
