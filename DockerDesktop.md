![Screenshot from 2024-11-02 22-28-40](https://github.com/user-attachments/assets/6089ed2f-2426-4765-ad57-ad75601f5f5d)

```bash
# Prints the same result as on the screenshot above
sudo ps aux | grep docker
```
```bash
# This command does't find any results
sudo systemctl list-units --type=service --all | grep docker
```

**Docker-Desktop is run as a process, not a service. The main process is `com.docker.backend`**

```bash
# To kill Docker-Desktop
sudo killall [-9] com.docker.backend
```
![Screenshot from 2024-11-02 22-52-10](https://github.com/user-attachments/assets/4673598a-7f47-4b32-b134-679f8cdb4fe8)
```bash
# To run Docker-Desctop from terminal
/opt/docker-desktop/bin/com.docker.backend
```

The `com.docker.backend` process is part of Docker Desktop for macOS and Windows. It is responsible for managing various backend operations of Docker Desktop, including interacting with the Docker daemon and handling the GUI elements of the application.

Socket location
```bash
/home/viktor/.docker/desktop/docker.sock
```

![Screenshot from 2025-02-13 22-38-37](https://github.com/user-attachments/assets/3116903a-28b1-4deb-a087-cf805a98ff88)

# Access Docker-Desktop underling VM

```bash
docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
```

- https://github.com/vikchupak/DevOps/blob/main/Linux/Virtualization/Docker.md#docker-desktop-on-linux-also-uses-vm
- https://techworld-with-nana.teachable.com/courses/1108792/lectures/42365903
