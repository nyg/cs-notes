# ACL

## Activation

Drive must be mounted with the `acl` options in `/etc/fstab` (already activated with `xfs`).

```
# <file system>            <mount point>    <type>    <options>       <dump>    <pass>
/dev/mapper/centos-root    /                xfs       defaults,acl    0         0
```

## Set ACL

```sh
# general syntax
setfacl -m [user|u|group|g]:<user|group>:rwx <file|folder>
```

##

The  `--set`  options set the ACL of a file or a directory, i.e. the previous ACL is replaced. The (non-ACL) file permissions must also be specified.

```sh
# set non-ACL permisions 000 for UGO and add rwx permissions for student user
$ setfacl --set u::-,g::-,o::-,u:student:rwx test
$ getfacl test
user::---
user:student:rwx
group::---
mask::rwx
other::---
```

The `-m` (`--modify`) option modify the current ACL of a file or directory.

```sh
# add read rights for root user
$ setfacl -m u:root:r test
$ getfacl test
user::---
user:root:r--
user:student:rwx
group::---
mask::rwx
other::---
```

The  `-x`  (`--remove`) option remove the specified ACL entries. The `-b`, `--remove-all` option removes all ACL entries, even default ones, but preserves non-ACL file permissions. The  `-k`, `--remove-default` option removes the default ACL.

```sh
# remove ACL for user root
$ setfacl -x u:root test
$ getfacl test
user::---
user:student:rwx
group::---
mask::rwx
other::---
```

The `-d` (`--default`) implies all operations apply to the default ACL. Regular ACL entries in the input set are promoted to Default ACL entries. Default ACL entries in the input set are discarded.

```sh
$ setfacl -d -m u:student:rwx testdir/
$ getfacl testdir/
user::rwx
group::rwx
other::r-x
default:user::rwx
default:user:student:rwx # <-
default:group::rwx
default:mask::rwx
default:other::r-x
[student@localhost ~]$ touch testdir/hello
[student@localhost ~]$ getfacl testdir/hello
user::rw-
user:student:rwx # <-     #effective:rw-
group::rwx                #effective:rw-
mask::rw-
other::r--
```

The `-R` (`--recursive`) applies operations to all files and directories recursively.

```sh
# set the specified default ACL on a/ and all of its subdirectories.
setfacl -R -d -m u:student:rwx a/
```
