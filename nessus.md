```sh
# Download link
# https://www.tenable.com/downloads/nessus

# Registration link
# https://www.tenable.com/products/nessus/nessus-essentials

# Install nessus
# https://docs.tenable.com/nessus/Content/InstallNessusLinux.htm
dpkg -i Nessus-<version number>-debian6_amd64.deb

# Start & stop
service nessusd start | restart | stop

# Uninstall nessus
# https://docs.tenable.com/nessus/Content/RemoveNessusLinux.htm
dpkg --purge nessus

# Erase registration and settings
# https://docs.tenable.com/nessus/commandlinereference/Content/ResetRegistrationAndEraseSettings.htm
/opt/nessus/sbin/nessuscli fix --reset
/opt/nessus/sbin/nessuscli fix --reset-all
```
