# LUKS

## Create encrypted volume

```sh
# create volume
pvcreate /dev/sdb
vgcreate vgdata /dev/sdb
lvcreate -l100%FREE vgdata -n lv_data

# encrypt volume
cryptsetup luksFormat /dev/vgdata/lv_data
cryptsetup luksOpen /dev/vgdata/lv_data lvcrypt

# format volume
mkfs.xfs /dev/mapper/lvcrypt
```

## `/etc/crypttab`

```sh
$ cat /etc/crypttab
lvcrypt	/dev/vgdata/lv_data	none
```

* The **first field** contains the name of the resulting encrypted block device; the device is set up within `/dev/mapper/`.
* The **second field** contains a path to the underlying block device or file, or a specification of a block device via "UUID=" followed by the UUID.
* The **third field** specifies the encryption password. If the field is not present or the password is set to "none" or "-", the password has to be manually entered during system boot. Otherwise, the field is interpreted as a absolute path to a file containing the encryption password.
* The **fourth field**, if present, is a comma-delimited list of options.

## `/cat/fstab`

```sh
$ cat /etc/fstab
/dev/mapper/lvcrypt    /secret    xfs    defaults    0    0
```

## Misc

```sh
dd if=/dev/zero of=/dev/mapper/lvcrypt status=progress
cryptsteup luksDump /dev/vgdata/lvdata
```
