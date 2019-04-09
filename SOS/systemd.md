# systemd

## Service example

```sh
$ cat /etc/systemd/system/httpd-docker-container.service
[Unit]
Description=httpd-docker-container

[Service]
Restart=always
ExecStart=/usr/bin/docker run --rm -i --name sos-httpd -p 8090:80 -v /home/student/web:/var/www/html:ro sos/httpd
ExecStop=/usr/bin/docker container kill sos-httpd

[Install]
WantedBy=multi-user.target
```

## Misc

```sh
# start or stop service
systemctl [start|stop] my-service

# make a service start after boot
systemctl enable my-service

# check service status
systemctl status my-service
```
