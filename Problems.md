# Docker Desktop DNS Problem

There might be a problem with dns while building images/containers:
- which leads to an infinite loading or couldn't resolve host error

The fix described here:\
https://stackoverflow.com/questions/24991136/docker-build-could-not-resolve-archive-ubuntu-com-apt-get-fails-to-install-a

![Screenshot from 2024-02-20 22-22-38](https://github.com/VIK2395/docker_commands/assets/50545334/e588213a-14fa-4251-8059-a9ce8d214412)

__Important: each vpn has its own dns ip, so we should add every vpn's dns ip to the config to freely switch between the vpns.__

# Volume mount problem (compose file)

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
