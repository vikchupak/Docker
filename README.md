# Docker cheatsheet

https://www.linux.org.ru/forum/desktop/16933835

There is no reall need in Docker Desktop for Windows/Liux if we have docker engine installed in wsl2(Ubuntu) via `apt` repository https://docs.docker.com/engine/install/ubuntu/ \
In this case, we need to install docker-compose separately https://docs.docker.com/compose/install/ as well via `apt` repository

https://learn.microsoft.com/en-us/windows/wsl/install \
https://learn.microsoft.com/en-us/windows/wsl/setup/environment \
https://docs.docker.com/engine/install/ubuntu/

Also, it is important to install node.js inside wsl2. Otherwise, we will try to run node.js project from windows wchich will result in errors.\
https://stackoverflow.com/questions/74000168/running-a-express-server-from-wsl-unc-paths-are-not-supported

__docker/docker-compose versions__\
https://docs.docker.com/engine/reference/commandline/version/ \
`docker version` - docker version\
`docker --version` - docker CLI version\
`docker compose version - `

__status/start/stop in wsl2__

`sudo service docker status`\
`sudo service docker start`\
`sudo service docker stop`

__Pull image:__\
`docker pull <image_name:[tag]>`

__Build an image from a Dockerfile:__\
`docker build -t <image_name:[tag]> <path_to_dockerfile> [--no-cache]`

__List images:__\
`docker images`

__Remove image__:\
`docker rmi <image_id or image_name>`\
`docker image rm <image_id or image_name>`

__List containers:__\
`docker ps (active)`\
`docker ps -a (all)`

__Create container:__\
`docker run -d --name <container_name> -p <host_port>:<container_port> -e <container_env_var_name>=<container_env_var_value> -v <host_volume_path>:<container_volume_path> --net=<network_name> <image_name:[tag]>`

- -d - detached mode;
- -it - interactive mode;
- --rm - to remove container after stop

_-d and -it are opposite_

__Start and stop container:__\
`docker start|stop <container_name or container_id>`

__Open a shell inside a running container:__\
`docker exec -it <container_name or container_id> sh|bash`

`exit` to quit;

__Attach to a container:__\
`docker attach <container_name or container_id>`

_The container logs will be seen, but it is not interactive mode;_

`Ctrl + C` _here will stop the container;_

__Watch mode:__\
`docker logs <container_name or container_id> -f`

`Ctrl + C` _here will NOT stop the container; Just stop wathing;_

__List docker networks:__\
`docker network ls`

__Create network:__\
`docker network create <network_name>`

__Connect a container to a network:__\
`docker network connect --ip <ip_to_assign_to_container> <network_name> <container_name or container_id>`

# Docker-compose

Important the project node_modules be installed from wsl2 with node.js installed in wsl2.
Otherwise there will be `Permission denied` errors.

Also, sudo can cause such errors.

__Run and stop existing containers:__\
`docker-compose start|stop`

__Build and start or stop and remove containers:__\
`docker-compose up|down [--build]`

- --build to force rebuild of the containers

__Work with contexts and builds:__\
`docker context ls`\
`docker context use <name>`\
https://docs.docker.com/build/builders/#selected-builder \
`docker buildx ls`\
`docker buildx use <name>`

__Note: when there are conteiners in different contexts, but using the same port, we will have an error 'port already in use'__

__Enable/disable on system startup:__\
https://askubuntu.com/questions/766318/disable-docker-autostart-at-boot \
https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot-with-systemd \
`sudo systemctl start docker`\
`sudo systemctl status docker`

`sudo systemctl disable docker.service`\
`sudo systemctl disable docker.socket`\
`sudo systemctl disable containerd.service`

Docker-desktop enable on boot: https://docs.docker.com/desktop/install/ubuntu/ \
`sudo systemctl --user enable docker-desktop`

![Screenshot from 2024-02-15 22-43-24](https://github.com/VIK2395/docker_commands/assets/50545334/a3c853e3-92f2-41d0-b578-a22c65dec244)

# Docker Desktop Problems
There might be a problem with dns while building images/containers:
- which leads to an infinite loading or couldn't resolve host error

The fix described here:\
https://stackoverflow.com/questions/24991136/docker-build-could-not-resolve-archive-ubuntu-com-apt-get-fails-to-install-a

![Screenshot from 2024-02-20 22-22-38](https://github.com/VIK2395/docker_commands/assets/50545334/e588213a-14fa-4251-8059-a9ce8d214412)

__Important: each vpn has its own dns ip, so we should add every vpn's dns ip to the config to freely switch between the vpns.__

# Docker Cleanup

https://stackoverflow.com/questions/44785585/how-can-i-delete-all-local-docker-images

__Stop all running containers:__\
`docker stop $(docker ps -q)`

- -q stands for "quiet", and is used to output containers' IDs only, instead of all the data

__Remove everything:__\
`docker system prune -a --volumes`

- About flags https://docs.docker.com/reference/cli/docker/system/prune/#extended-description
