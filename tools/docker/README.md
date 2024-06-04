# Basic overview of Docker

## Install Docker or Docker Desktop

### Installation on Windows

1. Install "Docker Desktop".
2. Once installed, it will need you to have also installed "Windows Subsystem Linux 2" (WSL2)
3. It has to be run as `administrator` user.
4. You can run from the `Windows Powershell` - again as `administrator`
5. Initially, working dirs will live in: `C:\Users\administrator\getting-started\` or other dirs in parallel.

### Installation on Linux

...will be simple...

## Create a Dockerfile

E.g. `getting-started/Dockerfile`:

```
# Get python
FROM python:3

ARG GIT_REPO
ARG GIT_HASH

# Make sure the apt-cache is up to date
RUN apt-get -y update

# Install git
RUN apt-get -y install git

# Get the repo
RUN git clone $GIT_REPO /app

# Checkout the specified version
RUN cd /app && git checkout $GIT_HASH

# Install requirements
RUN pip install -r /app/requirements.txt 
RUN pip install /app

# Run the application
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "flask_eg.flask_eg:app"] 
```

## Build the image

```
docker build . --build-arg GIT_REPO=https://github.com/cedadev/flask_eg --build-arg GIT_HASH=a268245 -t example/flask-app
```

## Run the image (and view in browser)

```
docker run -p 8080:5000 example/flask-app:latest
```

View at: http://localhost:8080/

## How do I stop a Docker image running from Powershell?

I don't think you can (stop it in Powershell)!

But **you can stop it using the Docker Desktop Dashboard** - by clicking the stop button in the "Containers/Apps" window.

## Or, run with interactive access (i.e. bash) to login to image

```
docker run -it example/flask-app:latest bash
```

## How to add a new port binding to a local Docker container that already exists (by making a copy)
You have already run and created a docker image, then you have exited it. List it first:

```
$ docker container ls --all
CONTAINER ID   IMAGE    COMMAND     CREATED      STATUS  PORTS       NAMES
14022c833b0a   con/ma3  "/bin/bash" 17 hours ago Exited              optimistic_ride
```

So, now I want to restart it and attach to it. Strangely, you need a new image, but you can do a rename like this.  Before doing `docker run` you can remove the original container and then assign the container the same name again:

```
$ docker stop 14022c833b0a
$ docker commit 14022c833b0a image01
$ docker rm 14022c833b0a
$ docker run -d --port=8181:8181 --name 14022c833b0a image01 /bin/bash
```

## How to restart a local container and connect interactively to it
```
docker start cranky_pascal

# Now use exec to login
docker exec -it cranky_pascal /bin/bash
```

## Deleting images and containers
List them with:
```
docker image ls

docker container ls --all
```

And delete as necessary, e.g.:
```
docker image rm --force 9efebc1fc367 fc4fde60d99e 77d64e4f353b

docker container rm --force 14022c833b0a brave_allen hardcore_curran jolly_rubin
```


