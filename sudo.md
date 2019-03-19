## sudo

* /etc/sudoers.d/* are concatenated to /etc/sudoers
* User does not need to belong to sudo group
* Add authorizations in config files, e.g.

```sh
$ cat /etc/sudoers.d/user2
user2 ALL = /usr/bin/yum install httpd, other cmds
```
