# OpenBSD

* [Awesome BSD](https://github.com/DiscoverBSD/awesome-bsd)
* [Awesome OpenBSD](https://github.com/ligurio/awesome-openbsd)

## Install

* [Installation guide](https://www.openbsd.org/faq/faq4.html)
* [Filesets](https://www.openbsd.org/faq/faq4.html#FilesNeeded)
* [Installation images](https://www.openbsd.org/faq/faq4.html#Download)

### Steps

* Choose (I)nstall
* Keyboard layout: en (or sf)
* System hostname
* Network interfaces to be configured
    * [em](https://man.openbsd.org/em)
    * [wpi](https://man.openbsd.org/wpi)
        * Need the firmware to be ready (could not read firmware wpi-3945abg)
        * https://superuser.com/a/832026/578935
    * [vlan](https://man.openbsd.org/vlan)
* DNS domain name: my.domain
* DNS nameservers: none
* Password for root account
* Start sshd by default: yes
* Start X Window System by xenodm: no
* Setup a user: yes
* Allow root ssh login: no
* Which disk is the root disk?
    * [wd](https://man.openbsd.org/wd)
    * [sd](https://man.openbsd.org/sd)
    * Disk setup: MBR, GPT, OpenBSD area, Edit
* Disk layout
* Location of sets: disk, is mounted: no
* Timezone: Europe/Paris

## Firmware

* Check for missing firmware with `fw_update -vi`.
    * https://man.openbsd.org/fw_update
* Install or update firmware for all drivers with `fw_update -a` (Internet connection required).
* Or download non-free firmware [here](https://firmware.openbsd.org/firmware/) and install them with `fw_update -p <dir>`, the `.tgz` firmware must be in the directory specified, no need to extract.

## Creating a bootable USB from macOS

1. Download `installXX.fs` (includes file sets, unlike `minirootXX.fs`)
2. Insert USB key
3. Get USB key device node `diskutil list` (e.g. `/dev/disk2`)
4. Unmount key: `diskutil unmountDisk /dev/diskX`
5. Write installer to key: `sudo dd if=installXX.fs of=/dev/rdiskX bs=1m`
6. macOS will say the key can’t be read but that’s okay.

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

* Edit cron jobs with `crontab -e`.
* List them with `crontab -l`.

    ```
    SHELL=/bin/sh
    PATH=/bin:/usr/bin:/usr/local/bin
    HOME=/var/log
    */5 * * * * /home/user/script.sh
    ```

## /etc/hostname.if

Config for DHCP and IPv6

```
dhcp
inet6 autoconf
inet6 alias <ipv6>
```

## /etc/sysctl.conf

* Config for IPv6?

    ```
    net.inet6.ip6.forwarding=0
    net.inet6.ip6.accept_rtadv=1
    ```

## Mounting a USB drive

Get the list of disks:

```sh
$ sysctl hw.disknames
hw.disknames=wd0:,cd0:,sd0:
```

Find out which partition to mount:

```sh
$ disklabel sd0
# /dev/rsd0c
type: SCSI
…

16 partitions:
# …
  i:    <size>    <offset>    <fstype>
```

Create the mount folder:

```sh
$ mkdir /mnt/folder
```

Mount the drive:

```sh
$ mount -t <fstype> /dev/sd0i /mnt/folder
```

* https://man.openbsd.org/sysctl.8
* https://man.openbsd.org/disklabel.8

## Misc

* `pkg_info -m` list of manually installed packages
* Set `pkg_path`?
* `/etc/installurl` must not be empty:
    * http://mirror.switch.ch/ftp/pub/OpenBSD
	* http://ftp.cc.uoc.gr/mirrors/OpenBSD
