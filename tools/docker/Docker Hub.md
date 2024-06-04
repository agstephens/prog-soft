See: https://hub.docker.com/

## List current images
```
$ docker image ls
REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
node            alpine    336ee465c313   12 days ago    142MB
alaniwi/daops   latest    b50a29268d56   8 months ago   2.02GB
rabbitmq        3.9.5     5a2ff76cc555   2 years ago    220MB
```
## Pull an image from a Docker Hub

```
$ docker pull alaniwi/docker101tutorial
```

## Run a local image
```
$ docker run alaniwi/docker101tutorial
```
List images (now something has changed):
```
$ docker image ls
REPOSITORY                  TAG       IMAGE ID       CREATED         SIZE
node                        alpine    336ee465c313   12 days ago     142MB
alaniwi/daops               latest    b50a29268d56   8 months ago    2.02GB
alaniwi/docker101tutorial   latest    c7ab0acd708a   11 months ago   47MB
rabbitmq                    3.9.5     5a2ff76cc555   2 years ago     220MB
```
## Remove an image from the local registry
```
docker image remove -f c7ab0acd708a
```
Where `-f` means `force`.
## Build a local image from the current directory and its Dockerfile
```
$ cd daops/
$ docker build . -t agstephens/daops-kerchunk:v0.1
```
## Push an image to Docker Hub
```
$ docker image push agstephens/daops-kerchunk:v0.1
```
NOTE: The `docker` command is hopefully already logged in to a Docker session and so authenticates itself to Docker Hub (at least in Windows `PowerShell`).
