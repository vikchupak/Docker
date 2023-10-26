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
`docker --version`\
`docker version`\
`docker compose version`

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

__WITHOUT sudo__. If sudo is used all folders/files are created under root user and no other users have access/permissions enough over files.\
Which causes  `Permission denied` errors.

__Run and stop existing containers:__\
`docker-compose start|stop`

__Build and start or stop and remove containers:__\
`docker-compose up|down [--build]`

- --build to force rebuild of the containers
