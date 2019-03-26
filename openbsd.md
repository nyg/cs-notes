# OpenBSD

## Creating a bootable USB from macOS

1. Download `installXX.fs` (includes file sets, unlike `minirootXX.fs`)
2. Insert USB key
3. Get USB key device node `diskutil list` (e.g. `/dev/disk2`)
4. Unmount key: `diskutil unmountDisk /dev/diskX`
5. Write installer to key: `sudo dd if=installXX.fs of=/dev/rdiskX bs=1m`
6. macOS will say the key can’t be read but that’s okay.

Links:
* https://superuser.com/questions/631592/why-is-dev-rdisk-about-20-times-faster-than-dev-disk-in-mac-os-x
* http://osxdaily.com/2015/06/05/copy-iso-to-usb-drive-mac-os-x-command
* https://www.openbsd.org/faq/faq4.html

## Partition layout

### One partition layout

1. Use (C)ustom layout
2. Delete all partitions with `d *`
3. Create swap with `a b` and with size e.g. 512M
4. Create main partition with `a` with mount point `/`

Info: partion b is swap, partition c is whole disk

### Bigger /usr for ports

1. To use whole disk: W for MBR, G for GPT
2. (E)dit auto layout
3. p g prints all partitions
4. R f resize auto allocated partition (f = /usr)
5. Input new size, e.g. 25g
6. q save & quit

## Relayd

* Daemon is `relayd`, control is `relayctl`
* Conf is at `/etc/relayd.conf`.
* Interesting command is `relayctl show summary`.
* `/etc/rc.conf.local` must be modified to start relayd at startup.
* `anchor "relayd/*”` must be added in  `/etc/pf.conf`.
* The forward IPv6 address is the local IPv4 address converted to IPv6 address:

    ```
    relay tcp6to4 {
        listen on <ipv6 address> port 80
        forward to 0:0:0:0:0:ffff:c0a8:65 port 8080 inet
    }
    ```

## Cron jobs

* Edit cron jobs with `crontab -e`
* List them with `crontab -l`.

    ```
    SHELL=/bin/sh
    PATH=/bin:/usr/bin:/usr/local/bin
    HOME=/var/log
    */5 * * * * /home/user/script.sh
    ```
