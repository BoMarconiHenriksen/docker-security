# Secure Docker Host

## Use a minimal operating system

- Ubuntu Core
- RancherOS
- CoreOS Depricated?

## Audit

Audit makes it possible to log and analyse all events on a system.  


## Linux Audit Framework

To get the latest version of Lynis <https://github.com/CISOfy/lynis> since the package version is not always the latest version.  

```bash
sudo apt-get install lynis
lynis audit system
```

## Create a New User
If you only has the root user then create a new user.  

```bash
useradd -m alexis -s /bin/bash
cat /etc/passwd #display user
passwd alexis # set password
passwd # Change root password
groups alexis # See which groups alexis is part of
usermod -aG sudo alexis # add alexis to sudo group
usermod -aG docker alexis # add alexis to the docker group
groups alexis
su alexis # switch user to alexis
cd ~
```

## SSH Key

Go to you desktop and create an SSH key.  

```bash
ssh-keygen -t ed25519 -C "My web-server key"  
ssh-copy-id alexis@ip-address # copy the public key to server
```

Go to your ubuntu server.  

```bash
sudo vim /etc/ssh/sshd_config
LogLevel VERBOSE # Get as much logging as possible
LoginGraceTime 2m
PermitRootLogin no
MaxAuthTries 2
MaxSessions 2
PasswordAuthentication no
PublicKeyAuthentication yes
ClientAliveCountMax 2

Save and exit
sudo systemctl restart ssh 
sudo lynis audit system
```

you can also setup failban.  

##

Clone docker bench if the user does not have it.  
```bash
git clone https://github.com/docker/docker-bench-security.git
cd docker-bench-security
sudo ./docker-bench-security.sh -c host_configuration
```

## Install AduitD

```bash
cd ~
sudo apt-get install auditd -y
sudo systemctl enable auditd # Starts up automatically when system reboot
sudo systemctl status auditd
sudo auditd -v
sudo aureport # a list of audit events
cd docker-bench-security
sudo ./docker-bench-security.sh -c host_configuration # See what rulkes you need to setup

sudo auditctl -w /run/containerd -k docker # -w = watch and -k = keyword for search
sudo auditctl -w /var/lib/docker -k docker
sudo auditctl -w /etc/docker -k docker
sudo auditctl -w /lib/systemd/system/docker service -k docker
sudo auditctl -w /lib/systemd/system/docker .socket -k docker
sudo auditctl -w /etc/default/docker -k docker
sudo auditctl -w /etc/docker/daemon.json -k docker
sudo auditctl -w /usr/bin/docker-containerd -k docker
sudo auditctl -w /usr/bin/docker-runc -k docker
sudo auditctl -w /us/bin/containerd -k docker
sudo auditctl -w /usr/bin/containerd-shim -k docker
sudo auditctl -w /usr/bin/containerd-shim-runc-v1 -k docker
sudo auditctl -w /usr/bin/containerd-shim-runc-v2 -k docker

dockerd -v
sudo aureport -k 

sudo audtctl -l # to see rules
# Copy the rules
sudo vim /etc/audit/audit.rules
# Insert the rules in the buttom of the file then save and exit.
sudo vim /etc/audit/rules.d/audit.rules
# Insert the rules in the buttom of the file then save and exit.

cd docker-bench-security
sudo ./docker-bench-security.sh -c host_configuration
sudo audit lynis system
```

More can be done here. See if it's possible to get a +70 score.  
