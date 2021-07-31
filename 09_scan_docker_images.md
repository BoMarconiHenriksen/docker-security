# Scan Docker Images

Docker vulnerability scanning is the process of identifying security vulnerabilities for packages utilized in a Docker image.  

## Trivy

<https://github.com/aquasecurity/trivy>  

```bash
docker pull aquasec/trivy
mkdir trivy
docker run --rm -v trivy:/root/.cache/ aquasec/trivy:0.19.2 [YOUR_IMAGE_NAME]
```

## Clair

<https://github.com/quay/clair>  

<https://github.com/arminc/clair-local-scan>  

```bash
docker run -d --name clair-db arminc/clair-db:latest
docker run -p 6060:6060 --link clair-db:postgres -d --name clair arminc/clair-local-scan:v2.0.8_fe9b059d930314b54c78f75afe265955faf4fdc1

docker ps -a # check that both images are running

# Go to Clair's Gihub page and click release and download the latest release
chmod +x # on the downloaded binary
ifconfig # Find the docker ip
./clair-scanner --ip 172.17.0.1 -r report.json ubuntu:18.04
```
