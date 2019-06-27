# iptables & co.

## Redirect port to another one

```sh
# redirect traffic going to port 80 to port 8080 on interface eth0
sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
```

## iptables

```sh
# save current rules to file
sudo iptables-save | sudo tee /etc/sysconfig/iptables

# overwrite current rules with the ones in the file
sudo iptables-restore < /etc/sysconfig/iptables

# add new rules but keep current ones
sudo iptables-restore -n < /etc/sysconfig/iptable
```

## SELinux

```sh
sudo yum install -y policycoreutils

# autorize listening on port 8080
semanage port -a -t http_port_t -p tcp 8080

# same result
semanage port -m -t http_port_t -p tcp 8080

# list all port definitions
semanage port -l
```

## Misc

```sh
# hide private IP addresses
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# show if port is being listened
sudo ss -tlpn | grep httpd

# see traffic from port 80 and interface eth0
tcpdump -i eth0 -n port 80
```
