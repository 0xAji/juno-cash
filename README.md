# Juno-Cash
if you run mining client on ubuntu, make sure you use 24.04, because juno-cash needed glibc 2.36

*docker image can run on every distro and version
## pre install
sudo apt update

## Mining
download latest mining client
```bash
wget https://github.com/juno-cash/junocash/releases/download/v0.9.1/junocash-0.9.1-linux-x86_64.tar.gz
```
extract client
```bash
tar xf junocash-0.9.1-linux-x86_64.tar.gz
```
copy the binary program to /usr/bin
```bash
cp ~/junocash-0.9.1-linux-x86_64/bin/junocashd
```
run client mining
```bash
junocashd -gen -genproclimit=-1 -minetolocalwallet
```
or use screen to run on background
```bash
screen -Rd juno
junocashd -gen -genproclimit=-1 -minetolocalwallet
```
use CTRL+A+D to exit from screen

change `genproclimit` to any thread you want to use

## Docker
run mining client with docker container
### pre install
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
### EXEC
```bash
docker run -d  --name ubuntu-juno  -v ~/.junocash:/root/.junocash 0xaji/juno:v1.0
```
```bash
docker exec -it ubuntu-juno /bin/sh
```
```bash
screen -Rd juno
```
```bash
junocashd -gen -genproclimit=-1 -minetolocalwallet
```
use CTRL+A+D to exit from screen, type `exit` to exit from docker
### SSH
```bash
docker run -d --name ubuntu-juno  -v ~/.junocash:/root/.junocash 0xaji/juno:v1.1
```
```bash
docker inspect ubuntu-juno | grep -i IPAddress
```
```bash
ssh root@IP -o PreferredAuthentications=password -o PubkeyAuthentication=no -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null
```
```bash
screen -Rd juno
```
```bash
junocashd -gen -genproclimit=-1 -minetolocalwallet
```
type `exit` to exit from ssh

all docker image using screen to run the mining client because when it run on entrypoint, mining client crashed, that's why my approach is through screen(exec/ssh)
