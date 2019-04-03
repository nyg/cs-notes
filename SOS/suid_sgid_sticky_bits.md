# SUID, GUID & Sticky bits

* [https://en.wikipedia.org/wiki/User_identifier](https://en.wikipedia.org/wiki/User_identifier)
* [https://en.wikipedia.org/wiki/Setuid](https://en.wikipedia.org/wiki/Setuid)
* [https://en.wikipedia.org/wiki/Sticky_bit](https://en.wikipedia.org/wiki/Sticky_bit)

## Description

### SUID & SGID on binaries

SUID (Set User ID) and SGID (Set Group ID) are special types of UNIX file permissions. When a binary is executed, it is so with the privileges of its owner (if SUID is set), of its group (if SGID is set) or of both (if both are set). These file permissions only works for binaries and are ignored for interpreted scripts.

A typical example is the `/usr/bin/passwd` which allows a user to change its password. Any user should be able to change its password, however, this requires `/etc/shadow` to be modified, which can only be done by root. By setting the SUID permission for the binary, which is owned by root, we allow this binary to be run as root (the *effective UID* is changed) when another user executes it.

SUID and SGID are different from `sudo` with which we can allow a user or group of users to execute a command as another user or group irrespective of who owns that binary. `sudo` is also used to determine who can execute a command, whilst with SUID and SGID we do not change who can execute a binary.

### SGID on directories

When the SGID bit is set on a directory, any file or directory created within that directory will inherit the group of the directory with the SGID bit, instead of the primary group of the user who created the directory.

### Sticky bit

Using the sticky bit on a directory prevents any user having write access to the directory from removing any files from that directory. Typically used on `/tmp`. On Linux it is ignored when set on files.

## Example

We have two users, user1 (UID 1001) and user2 (UID 1002). We also have a binary (`suid`) which reads a file (`protected_file`) and outputs its content. The binary can be executed by everybody but the file can be read only by user1. The binary is owned by user1.

For user2 to be able to see file's content when executing the binary, the SUID bit must be set, changing the effective UID from user2 (who can't read the file) to user1 (who can).

```c
/* suid.c */
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    printf("EUID=%d\n", geteuid());
    execlp("cat", "cat", "protected_file", 0);
    return 0;
}
```

User1 executes `suid` and the file's content is displayed:

```sh
# from user1
[user1@localhost tmp]$ cat protected_file
Hello from protected file!
[user1@localhost tmp]$ ls -l
total 20
-rw-rw----. 1 user1 user1   27 Mar 30 23:12 protected_file
-rwxrwxr-x. 1 user1 user1 8544 Mar 30 23:23 suid
-rw-rw-r--. 1 user1 user1  196 Mar 30 23:19 suid.c
[user1@localhost tmp]$ ./suid
EUID=1001
Hello from protected file!
[user1@localhost tmp]$
```

User2 tries the same thing but fails:

```sh
# from user2
[user2@localhost tmp]$ ls -l
total 20
-rw-rw----. 1 user1 user1   27 Mar 30 23:12 protected_file
-rwxrwxr-x. 1 user1 user1 8544 Mar 30 23:23 suid
-rw-rw-r--. 1 user1 user1  196 Mar 30 23:19 suid.c
[user2@localhost tmp]$ ./suid
EUID=1002
cat: protected_file: Permission denied
[user2@localhost tmp]$
```

User1 sets the SUID bit on the binary:

```sh
# from user1
[user1@localhost tmp]$ chmod u+s suid
[user1@localhost tmp]$ ls -l
total 20
-rw-rw----. 1 user1 user1   27 Mar 30 23:12 protected_file
-rwsrwxr-x. 1 user1 user1 8544 Mar 30 23:23 suid
-rw-rw-r--. 1 user1 user1  196 Mar 30 23:19 suid.c
[user1@localhost tmp]$
```

User2 can now see the file's content using the binary. We can see that the effective UID has changed and is now that of user1:

```sh
# from user2
[user2@localhost tmp]$ ls -l
total 20
-rw-rw----. 1 user1 user1   27 Mar 30 23:12 protected_file
-rwsrwxr-x. 1 user1 user1 8544 Mar 30 23:23 suid
-rw-rw-r--. 1 user1 user1  196 Mar 30 23:19 suid.c
[user2@localhost tmp]$ ./suid
EUID=1001
Hello from protected file!
[user2@localhost tmp]$
```
