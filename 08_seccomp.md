# Limit Container System Calls With Seccomp

Secure computing (seccomp) is a security feature in the Linux kernel that allows you to restrict the system calls that can be made by a process. In the context of containers seccomp works like a firewall for system calls and can be used to restrict the system calls made by Docker containers.  

A system call is the process through which a user-space process communicates with the Linux kernel in order to access resources or functionality. Whenever you want to create a file, change ownership or modify a network configuration, it is facilitated through the use of a system call.  

Docker has a default seccomp profile. <https://github.com/moby/moby/blob/master/profiles/seccomp/default.json>  

## Create Custom Seccomp Profile

Build a profile based on Dockers default profile.  

```bash
ls -alps /etc/seccomp/custom.json # Place your profile here

docker run -d --security-opt seccomp:/path/to/profile/profile.json <docker image>
```

## Disable Seccomp for a Container

```bash
 docker run -d --security-opt seccomp:unconfined <docker image>
```

## Monitor System Calls

This is done to dertermin how to create a profile.  

```bash
 docker run -d --security-opt apparmor:unconfined --security-opt seccomp:unconfined ubuntu:18.04 /bin/bash
apt update && apt install strace
strace -r chmod
```
