# auditctl

```sh
sudo auditctl -w /etc/passwd -p wra -k passwd
-w <file>        watch file
-p [read|write|execute|attribute]
-S <syscall>     observe systemcall
-a <list,action> e.g. exit,always
-k filter key (appears in logs)

# list rules
sudo auditctl -l

# delete all loaded rules
sudo auditd -D

# search audit log for syscalls
sudo ausyscall --dump | grep clock

# search key in logs
sudo ausearch -k changetime

# search actions for a specific file
sudo ausearch -f <file>
```
