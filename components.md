Docker engine
- Deamon | Server (`docker.service`, `dockerd` main process)
  - Container runtime (runs containers. `containerd.service`, `containerd` main process)
  - API (REST API)
  - Volumes
  - Network
  - Image builder (builds images)
- CLI

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
