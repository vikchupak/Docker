# Mount docker runtime from the Host to a container
```bash
# Mount docker.sock to allow the Jenkins instance inside the container to communicate with Docker on the host machine
docker run -p 8080:8080 \
-v jenkins_home:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
jenkins/jenkins:lts
```

```bash
# Inside contailer
# 1. Set correct permissions on docker.sock file
chmod 666 /var/run/docker.sock
# 2. Install docker CLI to enable you to run commands like docker build from within Jenkins
curl https://get.docker.com/ > dockerinstall && chmode 777 && ./dockerinstall
```

When you run `curl https://get.docker.com/ > dockerinstall && chmod 777 dockerinstall && ./dockerinstall` inside the Jenkins container, you’re actually installing the **Docker Engine**, which includes both the Docker **CLI** (client) and the **Daemon**. However, only the CLI is relevant and functional in this context because:

1. **The Docker Daemon in the Container Won't Start**: The container doesn’t have direct access to the low-level system resources required to run a second Docker daemon. Instead, it relies on the Docker daemon already running on the host, accessed via the socket at `/var/run/docker.sock`.

2. **Communication with Host Docker Daemon**: Mounting the Docker socket from the host (`/var/run/docker.sock:/var/run/docker.sock`) enables the Docker CLI inside the container to send commands to the Docker daemon on the host machine. The host's Docker daemon then handles container management based on these commands.

So, while the installation script technically installs both the Docker CLI and Docker daemon binaries, only the CLI is used. The Jenkins container essentially becomes a "client" that communicates with the Docker "server" (daemon) on the host, allowing it to run Docker commands without needing its own daemon.
