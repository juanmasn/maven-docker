# Docker image which includes maven + docker binaries
This image can be used as docker image to run maven builds and create docker images during that build.

As the maven:3-jdk-8 docker image does not include docker binaries 
``mvn package docker:build`` will fail. Therefor I modified the container and installed the docker binaries.

[This excellent article from jpetazzo](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci)
describes why it is not recommended to run docker within docker for CI servers. Instead he suggests to run docker with
``docker run -v /var/run/docker.sock:/var/run/docker.sock ...`` to give the container access to the Docker socket and 
therefor is able to start containers.



This can also be used with the gitlab runner executing the ci jobs. All we need to do is to update the `
``~/.gitlab-runner/config.toml`` file. The important part is the volume declaration.

```
[[runners]]
  ...
  [runners.docker]
    host = "tcp://192.168.99.100:2376"
    tls_cert_path = "/Users/twalter/.docker/machine/machines/default"
    tls_verify = true
    image = "twalter/mvn-docker"
    privileged = false
    volumes = ["/cache","/var/run/docker.sock:/var/run/docker.sock"]
```

## How to build this container?
```
docker build -t twalter/mvn-docker .
```

## How to run this container?
```
docker run -v /var/run/docker.sock:/var/run/docker.sock -it twalter/mvn-docker /bin/bash
```
