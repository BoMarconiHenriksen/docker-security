# Secure The Docker Deamon

## Control Who Has Access To The Docker Deamon

The user needs to be part of the docker group to inteact with docker.  

```bash
groups alexis
members docker
```

## Implementing TLS Encryption For Docker

This is **only nessesary if you have a remote docker host and want to connection to it via a client**. This is done with a tcp socket. Working locally on a server you connect with a domain socket.  

Implement TLS <https://github.com/AlexisAhmed/DockerSecurityEssentials>  

```bash
wget https://github.com/AlexisAhmed/DockerSecurityEssentials/blob/main/Docker-TLS-Authentication/secure-docker-daemon.sh
chmod +x secure-docker-daemon.sh
./secure-docker-daemon.sh
sudo chown -R root:root /home/alexis/.docker
sudo mkdir /etc/systemd/system/docker.service.d
sudo vim /etc/systemd/system/docker.service.d/override.conf

# Paste the below into the file and change the username
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D -H unix:///var/run/docker.sock --tlsverify --tlscert=/home/<user>/.docker/server-cert.pem --tlscacert=/home/<user>/.docker/ca.pem --tlskey=/home/<user>/.docker/server-key.pem -H tcp://0.0.0.0:2376

# Restart docker
sudo systemctl restart docker
netstat -antp
# Copy the key.pem, cert.pem and ca.pem to ~/.docker on the docker client and on the client(locally) set the following env variables
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST_IP="tcp://ip:2376"
export DOCKER_CERT_PATH="~/.docker"
```

## Implementing User Namespaces

The container is running as root. If there is a container break out the attacker will get root privileges. This is what we want to avoid.  

Docer creates an optional user called dockremap. So all containers should be runned by that user.  

```bash
cat /etc/subuid
sudo vim /etc/systemd/system/docker.service.d/override.conf

# Paste the below 
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D --userns-remap=”default”

# Restart docker deamon
sudo systemctl daemon-reload
sudo systemctl restart docker
cat /etc/subuid
```

## Intercontainer Communication

If your containers does not communicate with hitogther then you can enable this.  

```bash
sudo vim /etc/systemd/system/docker.service.d/override.conf

# Paste the below 
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D --userns-remap=”default” --icc=false

# Restart docker deamon
sudo systemctl daemon-reload
sudo systemctl restart docker

cd docker-bench-security
sudo ./docker-bench-security.sh
```
