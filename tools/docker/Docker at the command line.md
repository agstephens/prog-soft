## List containers
```
$ docker container ls --all
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS                                PORTS     NAMES
bfb0442f59ff   agstephens/ai-play   "/bin/bash"              6 weeks ago     Exited (137) Less than a second ago             fervent_satoshi
e5609f13a94b   1da9ab081e31         "/bin/bash"              7 months ago    Exited (0) 7 months ago                         competent_pasteur
1c98227e27b3   9efebc1fc367         "/bin/sh -c 'echo Co…"   9 months ago    Exited (0) 9 months ago                         practical_napier
b0f607c5dbc1   d8e3d5b66417         "bash"                   10 months ago   Exited (0) 10 months ago                        optimistic_mcclintock
```

## List images
```
$ docker image ls
REPOSITORY                  TAG       IMAGE ID       CREATED        SIZE
agstephens/ai-play          latest    7ac0816a0c85   2 months ago   4.07GB
continuumio/miniconda3      latest    1f702ef68adb   2 months ago   580MB
agstephens/daops-kerchunk   v0.4      88b795ace6a5   6 months ago   3.23GB
postgres                    11        60a93af0bba5   2 years ago    284MB
```
## Start an existing container
```
docker start bfb0442f59ff
```
## Connect to a bash session in a running container (as root)
```
$ docker exec -ti bfb0442f59ff /bin/bash
```
And then login as user from there:
```bash
su --shell /bin/bash - ag
```

## Sometimes, as an alternative to `start` and `exec` you might want to `run` and connect all in one
```bash
$ docker run -i -t  -p 8181:8181 continuumio/miniconda3 /bin/bash
```
## Change existing port bindings for a container
**You can't do this!**

But, you can do this (see [these instructions](https://www.baeldung.com/ops/assign-port-docker-container#relaunch-from-docker-commit)):
1. Stop the container and commit it as a new image.
2. Run a new container (from the saved image) with new port bindings.
3. Delete the old container and image.

1. Stop the container and commit it:
```bash
$ docker stop bfb0442f59ff
$ docker commit bfb0442f59ff agstephens/ai-play:v2
sha256:81e0d8a82682beda99c1a48e41b9231d5eb806d8aec4a250e93281290145ddcb
# This commits the container as an image - it can take a while (if multi-GB container)
```
2. Run a new container from that committed image:
```bash
$ docker run -i -t -p 8181:80 -p 8182:8000 agstephens/ai-play:v2 /bin/bash
```
3. Delete the old image and container:
```bash
$ docker image rm ???
$ docker container rm ???
```
## Rename an image (and tag)
```bash
$ docker image ls --all
REPOSITORY                  TAG       IMAGE ID       CREATED              SIZE
<none>                      <none>    81e0d8a82682   About a minute ago   12.3GB

$ docker image tag 81e0d8a82682 agstephens/ai-play:v2
```

### Copying a file into a running container
```
docker cp my-local/file.txt <container_id>:/tmp/hello.txt
```
