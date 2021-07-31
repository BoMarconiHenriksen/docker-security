# Docker Security

This is based on <https://www.youtube.com/watch?v=KINjI1tlo2w&list=PLBf0hzazHTGNv0-GVWZoveC49pIDHEHbn>  

## CVE Details

As a platform docker is pretty secure. Most of the security isuses comes from wrong configuration or the lack of understanding that you have to configure docker to be secure.  
<https://www.cvedetails.com/product/28125/Docker-Docker.html?vendor_id=13534>  

## Basic Commands

```bash
systemctl status # Check docker is running
docker system info # Your docker system configuration
docker version # Get latest version of docker
ls -alps /var/lib/docker/ # Where docker is located on host machine

docker image ls --digests # get image digest
```
