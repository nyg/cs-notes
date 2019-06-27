# Docker

## Dockerfile example

```
FROM centos:7
RUN yum update -y && yum install httpd httpd-tools -y
EXPOSE  80
CMD [ "/usr/sbin/httpd", "-D", "FOREGROUND" ]
```

## Mount volume and expose port

```
docker run -d -p 8080:80 -v <host folder>:<container folder>:<options> <docker image>
```
