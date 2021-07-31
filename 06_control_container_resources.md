# Controlling Container Resources Consumption With Control Groups

## Control Groups

Control Groups(cgroups) is a feature of the Linux kernel that is used to isolate, limit and account for resource usage on a systemfor a set of processes.  

In the case of Docker it limit resource consumptions for a container or a set of containers.  

## Tools

htop - monitor resources.  
docker stats - monitor container resource usages.  

## CPU Limits

```bash
docker run --cpus 0.25
```

## Total Amount of Cores

```bash
docker run --cpuset-cpus=1
```

## Max Usage Of RAM

```bash
docker run -m 128m
```

## Limit Total Amount of PID(Process ID's) That Can Run

```bash
docker run --pids-limit 5
```

## Modify Resource Consumtion for a Running Container

```bash
docker ps # get the container id
docker update --cpus 0.25 <container id>
```
