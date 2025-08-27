## Tagging a Docker image

```
docker tag registry.ceda.ac.uk/ukcp-wps/webserver:latest
```

## Push to the CEDA registry (Harbor)

First login:

```
docker login registry.ceda.ac.uk
```

Push to the registry:

```
docker push registry.ceda.ac.uk/ukcp-wps/webserver:latest
```

Look in Harbor:

https://registry.ceda.ac.uk/harbor/projects/3249/repositories/webserver/artifacts-tab/artifacts/sha256:c2cca8a8a6b08db01182f83359879f0bab62a93e835166644f9df3249291c2dc?sbomDigest=

## See all the commands that built an image

```
docker image history
```

## You can `dive` into a history of layers

```
dive quay.io/jupyter/scipy-notebook:latest
```

