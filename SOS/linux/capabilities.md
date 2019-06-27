# Capabilities

```sh
# show current capabilities
getcap ping

# set capabilities on ping
setcap cap_net_raw+ep ping

# remove all capabilities on ping
setcap -r ping
```
