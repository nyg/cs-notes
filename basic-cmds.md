## Basic commands

### `sort`, `uniq`, `cut`

```sh
# Sort the first column of <file> in numerical reverse order (-k1gr) and then
# the third column alphabetically, using ':' as field separator.
cat <file> | sort -t':' -k1gr -k2d
```

### `grep`, `sed`, `awk`

* In-place file modification: `sed -i -E 's/^(SELINUX=).*$/\1disabled/' /etc/sysconfig/selinux`

### Archives

* `zip -r <zip file> <file>...`
* `tar -xzf bar.tar.gz -C <dest>`

### Misc

* `scp -P <port> <source> <target>`, e.g. `user@host:path`
* `mktemp [-d] /tmp/file_XXXX`, create files or directory with unique names
