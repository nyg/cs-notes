# snort

## Configuration

* Default configuration is in `/etc/snort/snort.config`.
* A configuration file includes `.rules` file.
* Rules files are in `/etc/snort/rules/icmp2.rules`.
* Start with `snort -c /etc/snort/my-snort.conf`.

## Rules

* A rule must be define on one line only.
* Split into the header and options parts.

```
# creates an alert for each ICMP packet from and to any IP and port
# the alert contains the message "ICMP Packet"
alert icmp any any -> any any (msg: "ICMP Packet"; sid: 4000001; rev: 1;)
```

## Actions

* Alerts in `/var/log/snort/alerts` file.
* Logs in `/var/log/snort/snort.log.<timestamp>`. File is in pcap format, readable by tcpdump or Wireshark.
