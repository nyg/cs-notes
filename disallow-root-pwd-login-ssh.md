## Disallow password login for root with ssh

### Client

1. create key: `ssh-keygen -t rsa -b 4096`
2. add key config in .ssh/config
3. use ssh-copy-id to copy the public key to the remote machine authorized-keys file.

### Server

1. edit /etc/ssh/ssdh_config: PermitRootLogin without-password
2. restart sshd: service sshd restart
