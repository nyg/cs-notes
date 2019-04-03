# mount

## Misc

```sh
# info on devices
fdisk -l
blkid

# mount device in dir with the given filesystems
mount [-t fs-type] [device] <dir>
-a  mount all filesystems in fstab
-o  specify options, e.g. remount, noexec, ro
    display all mounted filesystem

# unmount filesystem
umount <dir|device>
```

## Options

* **nodev**: do not interpret character or block special devices on the file system.
* **noexec** : do not allow direct execution of any binaries on the mounted filesystem.
* **nosuid** : do  not  allow set-user-identifier or set-group-identifier bits to take effect.
