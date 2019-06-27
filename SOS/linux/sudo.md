# sudo

## Configuration

* Configuration files in: `/etc/sudoers` and `/etc/sudoers.d/*`.
* Use `visudo` to edit configuration files, to ensure correct syntax.

```sh
# from ALL machines, user2 can install httpd using yum
user2 ALL = /usr/bin/yum install httpd

# allow all users in group web to restart httpd without having to enter their password
%web ALL = NOPASSWD: /usr/bin/systemctl restart httpd

# all user student to use touch and mkdir as root:root
student ALL = (root:root) /usr/bin/touch, /usr/bin/mkdir

# specify PATH where running sudo command
Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin

# allow user2 to run vim as root but preventing vim from executing other commands
user2 ALL = (root:root) NOEXEC: /usr/bin/vim

# adds /data to PATH for user2
Defaults:user2 secure_path += :/data
```

## Commands

```sh
sudo
-s run default shell of specified user
-i same but as a login shell
-g specify group with which to run sudo
-u specify user to impersonate
-e edit a file instead of running a command, same as sudoedit

# omits #include directive
$ sudo cat /etc/sudoers | grep -E '^[^\s#]'
```
