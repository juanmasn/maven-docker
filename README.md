# Docker image which includes maven + docker binaries
This image can be used as docker image to run maven builds and create docker images during that build.

## How to build this container?
```
docker build -t twalter/mvn-docker .
```

## How to run this container?
```
docker run -v /var/run/docker.sock:/var/run/docker.sock -it twalter/mvn-docker /bin/bash
```
