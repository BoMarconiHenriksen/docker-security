# Implementing Access Control With App Armor

Limit what our containers have access to and thereby making them more secure.  

AppArmor is used to manage access to OS resources by utilizing custom profiles for applications and containers.  

Access control is the process of managing and controlling access to a system resource. In the context of containers, we need to configure what system resources and functionality the containers can access.  

Limits the tools the container have access to so it only get access to what is needed.  

1. Discretionary Access Control: Access to resources is specified by the resource owner. An example of this is the implementation of file and directory permissions. This type of access control does not offer much in regard to the types of resources we can restrict access to.   
2. Mandatory Access Control: Access to resources is dependent on predefined access policies. Examples of solutions that provide mandatory access control are SELinux and AppArmor.  

Docker has a default AppArmor.  

For this we are using Bane that can create an AppArmor profile.  

## Check If AppArmor is Installed

```bash
aa-enabled # check if appArmor is enabled
ls -alp /etc/apparmor.d/ # where to add appArmor profiles
```

## Bane

<https://github.com/genuinetools/bane>  

```bash
# Go to Gihub repo and click release and go to arm64 - linux
# Export the sha256sum for verification.
$ export BANE_SHA256="67f899de5e19d1dd072e5b3f52af97af59803b2394dd09099728c6399dd42093"

# Download and check the sha256sum.
$ curl -fSL "https://github.com/genuinetools/bane/releases/download/v0.4.4/bane-linux-arm64" -o "/usr/local/bin/bane" \
	&& echo "${BANE_SHA256}  /usr/local/bin/bane" | sha256sum -c - \
	&& chmod a+x "/usr/local/bin/bane"

$ echo "bane installed!"

# Run it!
$ bane -h
```

## Create a Profile

<https://github.com/genuinetools/bane/blob/master/sample.toml>

```bash
docker pull nginx
cd /etc/apparmor.d
sudo vim nginx-secure.toml # paste the sample.toml in this file then save and exit
sudo bane nginx-secure.toml # installs the profile in AppArmor
docker run -d --security-opt="apparmor:docker-nginx-secure" nginx

sudo aa-status # See what appArmor are enforced
```

## Run a Container Without AppArmor

```bash
docker run -d --security-opt=apparmor:unconfined ubuntu:18.04 /bin/bash
```
