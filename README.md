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

_This is NOT interfering with the main process, but starts a new one additional._\
_It is like openning a new terminal inside a container_

`exit` to quit;

__Attach to a container:__\
`docker attach <container_name or container_id>`

_The container logs will be seen, but only newly generated after attaching, but it is not interactive mode; This like undo -d flag in `docker run -d ...`_\
_This connects to a running container’s __main process__ allowing you to interact with its output (stdin, stdout, stderr)_

`Ctrl + C` _here will stop the container;_

__Watch mode:__\
`docker logs <container_name or container_id> -f`

_ALL container logs will be seen from very container start_

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

# Volume mount problems (compose file)

This creates directories on host instead of files \
![Screenshot from 2024-08-12 19-01-00](https://github.com/user-attachments/assets/43a5521a-ae25-4f7a-b4a4-4ff436850134) \
![Screenshot from 2024-08-12 19-06-29](https://github.com/user-attachments/assets/3513ad13-b417-4b9d-b7ef-0f5d4c96bf54)

Problem explanation \
The reason is no real files on the host (or in containers) when containers are being created \
https://stackoverflow.com/questions/34134343/single-file-volume-mounted-as-directory-in-docker

Solution \
Create files on the host before runnung the containers run.

![Screenshot from 2024-08-12 19-20-02](https://github.com/user-attachments/assets/476b5f2d-8d15-4f78-a8e4-ddbdac0369f4) \
![Screenshot from 2024-08-12 19-20-43](https://github.com/user-attachments/assets/139e91ff-5359-48d8-b86a-6c750f1f44f4)

The following have not worked \
https://stackoverflow.com/questions/42248198/how-to-mount-a-single-file-in-a-volume

# Docker Cleanup

https://stackoverflow.com/questions/44785585/how-can-i-delete-all-local-docker-images

__Stop all running containers:__\
`docker stop $(docker ps -q)`

- -q stands for "quiet", and is used to output containers' IDs only, instead of all the data

__Remove everything:__\
`docker system prune -a --volumes`

- About flags https://docs.docker.com/reference/cli/docker/system/prune/#extended-description

# CMD vs ENTRYPOINT

In Docker, both `CMD` and `ENTRYPOINT` define the default commands that are executed when a container starts. However, they have slightly different purposes and behavior. Understanding their differences helps in determining how to structure your Dockerfile based on the intended behavior of the container.

### 1. **CMD**
- **Purpose**: The `CMD` instruction specifies the default command to be run when the container starts. It's mainly intended to provide **default arguments** for the `ENTRYPOINT` command or act as the main command if `ENTRYPOINT` is not specified.
- **Behavior**:
  - If the user provides a command when starting the container (e.g., `docker run myimage <command>`), the command specified in `CMD` will be **overridden**.
  - If you provide multiple `CMD` instructions in a Dockerfile, only the last one will take effect.

- **Example**:
  ```Dockerfile
  FROM ubuntu
  CMD ["echo", "Hello, World!"]
  ```

  In this example:
  - When the container runs, it will execute `echo "Hello, World!"` by default.
  - If you run `docker run myimage ls`, it will execute `ls` instead of the default `CMD` because the command `ls` overrides `CMD`.

- **Formats**:
  - **Shell form**: `CMD echo "Hello, World!"`
  - **Exec form**: `CMD ["echo", "Hello, World!"]`

### 2. **ENTRYPOINT**
- **Purpose**: The `ENTRYPOINT` instruction specifies the main executable that will always run when the container starts. It is used when you want the container to behave like an executable, where any additional arguments passed to `docker run` are treated as arguments to the `ENTRYPOINT` command.
- **Behavior**:
  - Unlike `CMD`, `ENTRYPOINT` **cannot be easily overridden** by arguments passed to `docker run`. The command passed will be treated as arguments to the `ENTRYPOINT` command.
  - You can override `ENTRYPOINT` at runtime using the `--entrypoint` flag in `docker run`.
  - You can combine `ENTRYPOINT` with `CMD` to provide **default arguments**.

- **Example**:
  ```Dockerfile
  FROM ubuntu
  ENTRYPOINT ["echo"]
  CMD ["Hello, World!"]
  ```

  In this example:
  - When the container runs, it will execute `echo "Hello, World!"` by default.
  - If you run `docker run myimage Goodbye`, it will execute `echo "Goodbye"`, where `"Goodbye"` overrides the `CMD` argument but not the `ENTRYPOINT`.

- **Formats**:
  - **Shell form**: `ENTRYPOINT echo "Hello, World!"`
  - **Exec form**: `ENTRYPOINT ["echo"]`

### **CMD vs ENTRYPOINT Comparison**

| Aspect                | CMD                              | ENTRYPOINT                         |
|-----------------------|----------------------------------|------------------------------------|
| Purpose               | Provides default arguments or a command to run. | Defines the main executable that should always run. |
| Overridable?          | Yes, can be overridden by `docker run <command>`. | No, unless overridden by `--entrypoint` option. |
| When to use?          | Use when the command can be overridden frequently. | Use when you want a fixed command with optional arguments. |
| Combination with the other? | Can be combined with `ENTRYPOINT` to supply default arguments. | Can be combined with `CMD` to provide default arguments. |

### **Combining ENTRYPOINT and CMD**

You can use both `ENTRYPOINT` and `CMD` in a Dockerfile. In this case:
- `ENTRYPOINT` defines the fixed command that will run.
- `CMD` provides default arguments to the `ENTRYPOINT` command.

**Example**:
```Dockerfile
FROM ubuntu
ENTRYPOINT ["curl"]
CMD ["http://example.com"]
```
- When you run `docker run myimage`, it will execute `curl http://example.com`.
- If you run `docker run myimage http://google.com`, it will execute `curl http://google.com`, where `http://google.com` replaces the `CMD` argument but not the `ENTRYPOINT`.
