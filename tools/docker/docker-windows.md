# An example of running a container on Windows

## Example

Using this recipe (_Hands On Machine Learining_ book):

https://github.com/ageron/handson-ml3/tree/main/docker

## Pre-requisites

Install `Docker Desktop` on Windows. It will install the required command-line tools.

## In Windows Powershell

As `administrator`, run Powershell and do:

```
cd C:\
mkdir "Docker files"

cd "Docker files"

git clone https://github.com/ageron/handson-ml3
cd handson-ml3/

# One-off - builder the Docker file (or alternatively you could pull it from DockerHub)
docker-compose build

# Run it with
docker-compose up

# It will run and give you a URL to go to in order to access a Notebook service in the container
```



