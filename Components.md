- https://docs.docker.com/engine/
- https://docs.docker.com/engine/daemon/start/
- https://docs.docker.com/build/concepts/overview/ (Docker Build. We can create multiple builders per a context. Each builder in buildx is associated with a specific Docker context)
- https://docs.docker.com/build/builders/#selected-builder
- https://docs.docker.com/engine/manage-resources/contexts/ (Contexts are used to switch between different deamons that still use the same client)

Docker engine
- Deamon | Server (`docker.service`, `dockerd` main process)
  - API (REST API throuch which the CLI client talks to Docker server)
  - Container runtime (runs containers. `containerd.service`, `containerd` main process)
  - Volumes
  - Network
  - Image builder (builds images, BuildKit backend | server && Buildx client)
- CLI (Client, uses Docker REST API to talk to Docker Deamon | Server)

```bash
 # systemctl status docker

● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2024-11-02 21:19:30 EET; 1min 42s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 404 (dockerd)
      Tasks: 88
     Memory: 237.9M
     CGroup: /system.slice/docker.service
             ├─ 404 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
             ├─1059 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3306 -container-ip 172.20.0.2 -container-port 3306
             ├─1070 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 3306 -container-ip 172.20.0.2 -container-port 3306
             ├─1080 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 6379 -container-ip 172.18.0.2 -container-port 6379
             ├─1087 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 6379 -container-ip 172.18.0.2 -container-port 6379
             ├─3600 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3046 -container-ip 172.18.0.3 -container-port 80
             └─3607 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 3046 -container-ip 172.18.0.3 -container-port 80
```
