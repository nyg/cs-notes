# Misc

```sh
# create 2048-bit key (??)
sudo dd if=/dev/urandom of=/root/secret.key bs=1024 count=2

# find all files and folders owned by root with SUID
sudo find / -perm /4000 -user root 2>/dev/null
```
