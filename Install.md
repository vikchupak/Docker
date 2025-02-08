https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

On a brand-new machine, quick user create
- https://github.com/vikchupak/DevOps/blob/main/Linux/addUser.md#create-a-new-user-non-interactive

# Using snap

Compose is included
```bash
sudo snap install docker
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot
```
