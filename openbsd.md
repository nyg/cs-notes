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

Example for a single partition layout.

1. Use (C)ustom layout
2. Delete all partitions with `d *`
3. Create swap with `a b` and with size e.g. 512M
4. Create main partition with `a` with mount point `/`

Info: partion b is swap, partition c is whole disk
