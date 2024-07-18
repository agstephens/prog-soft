# An example of running a container on Windows

## Example

Using this recipe (_Hands On Machine Learning_ book):

https://github.com/ageron/handson-ml3/tree/main/docker
## Pre-requisites

Install `Docker Desktop` on Windows. It will install the required command-line tools. 

Start `Docker Desktop` as `Administrator`.
## In Windows PowerShell
Run PowerShell, then use `docker` at the command-line, e.g.:

```
# List all containers
docker container ls

# List all images
docker image ls
```

```
# Pull a remote image on DockerHub
docker pull continuumio/miniconda3

# Run a container from that image, and login to bash shell
docker run -i -t  -p 8181:8181 continuumio/miniconda3 /bin/bash

# !!!NOTE: Running the above three times will CREATE 3 SEPARATE CONTAINERS!!!
```

```
# Start a container with:
docker start bfb0442f59ff

# Or stop it with
docker stop bfb23424898e

# If it is running, connect to a bash shell with
docker exec -ti bfb0442f59ff /bin/bash
```

## Building from a `docker-compose.yml` file
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

### Setting up a non-root user in a container
```
# Once logged in as root
mkdir /home/ag
useradd -d /home/ag ag
chown ag.users /home/ag

# Login as: ag
su --shell /bin/bash - ag

cd ~

# Create the .profile file
echo "source /opt/conda/etc/profile.d/conda.sh" > .profile
echo "conda activate base" >> .profile

echo "syntax off" > .vimrc
echo "set paste" >> .vimrc

mkdir -p venvs

exit

# Login again and the conda base env is sourced
su --shell /bin/bash - ag
```



