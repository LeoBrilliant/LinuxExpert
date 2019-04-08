
# registry
## local registry
$ docker run -d -p 5000:5000 --restart=always --name registry registry:2
$ docker pull ubuntu:16.04
$ docker tag ubuntu:16.04 localhost:5000/my-ubuntu
$ docker push localhost:5000/my-ubuntu
$ docker image remove ubuntu:16.04
$ docker image remove localhost:5000/my-ubuntu
$ docker pull localhost:5000/my-ubuntu

$ docker container stop registry
$ docker container rm -v registry

- https://docs.docker.com/registry/deploying/#copy-an-image-from-docker-hub-to-your-registry

## set up private registry with self signed certificate
$ sudo vim /etc/ssl/openssl.cnf # add subjectAltName to [v3_ca] section, this is important.
[v3_ca]
subjectAltName = IP:192.168.0.19

$ openssl req -newkey rsa:4096 -nodes -sha256 -keyoput certs/domain.key -x509 -days 365 -out certs/domain.crt
...
Country Name (2 letter code) [AU]: CN
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:192.168.0.19
Email Address []:

$ docker run -d -p 5000:5000 --restart=always --name registry -v /home/bliu/workspace/docker/certs:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key registry:2
$ docker ps 
$ docker logs registry

## access the private registry
$ sudo mkdir -p /etc/docker/certs.d/192.168.0.19:5000
$ sudo cp -v /home/bliu/workspace/docker/certs/domain.crt /etc/docker/certs.d/192.168.0.19:5000/ca.crt
$ sudo service docker reload  # this looks optional
$ docker tag ubuntu:16.04 192.168.0.19:5000/my-ubuntu
$ docker push 192.168.0.19:5000/my-ubuntu

### x509: certificate signed by unknown authority
- see <access the private registry>

### x509: cannot validate certificate for … because it doesn’t contain any IP SANs .
- see <set up private registry with self signed certificate>

- https://github.com/docker/distribution/issues/948
- https://hackernoon.com/create-a-private-local-docker-registry-5c79ce912620
- https://ralph.blog.imixs.com/2017/04/22/how-to-setup-a-private-docker-registry/
- https://stackoverflow.com/questions/26026931/setting-up-a-remote-private-docker-registry
- https://blog.csdn.net/hezuohuoban882/article/details/76170156

# dial unix /var/run/docker.sock: connect: permission denied.
$ sudo usermod -a -G docker $USER
- https://techoverflow.net/2017/03/01/solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/

# docker run
$ docker run -i -t foo /bin/bash

# docker attach 
$ docker attach <container name>
- https://blog.csdn.net/u010397369/article/details/41045251

# detach from a container without stopping it
Ctrl-p, Ctrl-q
or set the detach keys when attaching 
$ docker attach --detach-keys="ctrl-a,ctrl-x" condescending_shirley
- https://stackoverflow.com/questions/25267372/correct-way-to-detach-from-a-container-without-stopping-it

# How to backup and restore the registry
$ docker volume create my-vol
$ docker run -d -p 5000:5000 --restart=always --name registry \
     -v /home/bliu/workspace/docker/certs:/certs \
     -v my-vol:/var/lib/registry \
     -e REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/var/lib/registry \
     -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
     -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
     registry:2

$ docker push 192.168.0.19:5000/my-ubuntu

$ rsync -avz -P /var/lib/docker/volumes/my-vol/ /var/lib/docker/volumes/bak-vol/ # we can tar the bak-vol if necessary
$ docker container stop registry
$ $ docker run -d -p 5000:5000 --restart=always --name registry \
     -v /home/bliu/workspace/docker/certs:/certs \
     -v my-vol:/var/lib/registry \
     -e REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/var/lib/registry \
     -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
     -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
     registry:2

$ docker pull 192.168.0.19:5000/my-ubuntu

- http://jgsqware.github.io/2016/02/docker-registry-installation/   # gives another way to backup registry, using a container to do the job.
- https://docs.docker.com/storage/volumes/#create-and-manage-volumes

## commit save image
$ docker commit register register:backup
$ docker save register:backup -o register_backup.tar
$ docker load -i register_back.tar
- https://www.thegeekdiary.com/how-to-backup-and-restore-docker-containers/

## check the size of volume
$ docker system df -v
- https://docs.docker.com/engine/reference/commandline/system_df/

# list repositories and images in the registry
$ curl -X GET https://192.168.0.19:5000/v2/_catalog  #_/
$ curl --cacert domain.crt https://192.168.0.19:5000/v2/my-ubuntu/tags/list

- https://stackoverflow.com/questions/31251356/how-to-get-a-list-of-images-on-docker-registry-v2

# harbor
- https://github.com/goharbor/harbor/tree/master/docs
- https://github.com/goharbor/harbor/blob/master/docs/user_guide.md

# Access host port from docker container
- https://stackoverflow.com/questions/31324981/how-to-access-host-port-from-docker-container
