# Securing Containers During Runtime

```dockerfile
FROM ubuntu: 18.04

# Add user that does not have root privileges - Add sudo after -g to add sudo priviliges
RUN groupadd -r bob && useradd -r -g bob bob

# Environment Variables
ENV HOME /home/bob
ENV DEBIAN_FRONTEND=noninteractive
```

Build the image.  

```bash
docker build . -t test
docker images
docker run -u bob -it --rm 3345rdefg /bin/bash
su root
exit and docker rmi id
```

## Block Access in Container to Root

```dockerfile
FROM ubuntu: 18.04

# Add user that does not have root privileges - Add sudo after -g to add sudo priviliges
RUN groupadd -r bob && useradd -r -g bob bob
# Make sure that you can not switch to the root user in the conatiner
RUN chsh -s /usr/sbin/nologin root

# Environment Variables
ENV HOME /home/bob
ENV DEBIAN_FRONTEND=noninteractive
```

Build the image.  

```bash
docker build . -t test
docker images
docker run -u bob -it --rm 3345rdefg /bin/bash
su root
exit and docker rmi id
```

docker run -u bob --security-opt=no-new-privileges <IMAGE-ID>

## Kernal Capabilities

Remove all capabilities and then add those that are needed.  

```bash
docker run --cap-drop all cap-add NET_ADMIN
```

## Restrict access to the filesystem

You can make a container run in read only mode.  

```bash
docker run --read-only -u bob
```

Create a temporary filesystem where you can write

```bash
docker run --read-only --tmpfs /opt
```

## Isolate Container Communication

In order to disable inter-container communication, we will need to create a new Docker network. This can be done by running the following command with the “icc” option set to false.  

```bash
docker network ls
docker network inspect # All containers has access to bridge mode

docker network create --driver bridge -o “com.docker.network.bridge.enable_icc”=”false” test-net
docker network ls
docker network inspect test-net
```
Even if containers are on the same network they can not communicate.  
