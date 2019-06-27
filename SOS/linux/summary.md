# SOS

## Concepts Linux

### Commandes de base & fichiers de config

#### Users, groups et mots de passe

* /etc/passwd, /etc/shadow
* /etc/group, /etc/gshadow

#### Processus

* ps, pstree, kill, pkill, pgrep, htop

#### Matériel

* lsusb, lspci, lshw, dmidecode
* /proc/cpuinfo, /proc/partitions

#### Drives

* mount, umount, df, fdisk, parted, cfdisk, fsck, dd, mdadm, lsblk, blkid
* ip, ss, ping, nslookup, arp, ssh

#### Services

* systemctl, service

## Outils de consolidation

### Mots de passe

* Hashcat, JohnTheRipper
* passwordmaker**, keepass[xc]
* Pluggable Authentication Module (PAM)
	* /etc/pam.d
	* Module pam_pwquality (ex. pam_cracklib)
		* yum install libpwquality ou
		* vim /etc/pam.d/{password,system}-auth
		* /etc/security/pwquality.conf
* pwscore, cracklib-check

### UNIX access rights

```
-rwsrwSrwx    SUID and user executable, SGID but not group executable
-rwxrwxrwxt   Sticky bit
-rwxrwxrwx+   ACL
-rwxrwxrwx.   SELinux security context
```

* Access rights are changed with `chmod`. Owner and group with `chown`.
* **Set User ID** (SUID), **Set Group ID** (SGID): when executed, a binary is done so with the privileges of its owner and/or group. Only works for binaries.
	* set SUID with `u+s` or `4xxx`; SGID with `g+s` or `2xxx`
* **Sticky bit**: only root or owner of directory can remove files from said directory. Typically used on `/tmp`.
* **Access Control List** (ACL):
	* drive must be mounted with the `acl` options in `/etc/fstab`, already the case with xfs.
	* `setfacl`, `getfacl`
		* `setfacl -m [user|u|group|g]:<user|group>:rwx <file>`, `-m` modify, `-d` default for new files.
* **POSIX Extended attributes**
	* `lsattr`, `chattr`
	* `iAcs`, immutable file; atime not modified; compressed; secure deletion; etc.
* **POSIX capabilities**
	* les droits de l'utilisateur root sont décomposables en *capabilities* et *privilèges*.
	* possibilité de modification par processus et/ou fichiers.
	* `getcap <file>`
	* remove all capabilities: `setcap -r <file>`
	* add the specified capabilities: `setcap <capabilities> <file>`

Les droits UNIX définissent quels appels système peuvent accéder à quels fichiers.

## Limitation des opportunités vulnérabilités

* Informations disponibles à propos d'un serveur
	* /proc, /dev, /sys, kernel.dmesg_restrict
	* liste des processus avec pid, hidepid=0,1,2
* Réduire le nombre de services tournant sur le serveur
	* ou limité l'accès, firewall, execution, isolation
* Bon choix d'applications
* Mount avec noexec, nodev, nosuid

## Supervision et contrôle d'intégrité

* Comment savoir qu'un serveur a été compromis
	* auditd, tripwire, analyse de logs (centralisés), surveillance du système
	* checksum des binaires et fichiers importants (AIDE)
	* Analyse de logs en temps réel
	* Analyse des processus en cours d'exécution
	* Activité des utilisateur privilégiés
	* Analyse DNS, comportement réseaux
	* etc.
* Outils de supervision
	* systemd-journald
	* OSSEC, Snort, SAMHAIN, Prelude (IDS)
	* Nagios, Icinga, Shinken, Zabbix (Monitoring)
	* Splunk, Logstash (Logs)
* Logs dans /var/log/*
* Outils d'intégrité
	* Linux Integrity Subsystem, Linux-IMA
	* AIDE, Auditd, OSSEC, Tripwire
* AIDE, Advanced Intrusion Detection Environment
	* Contrôle les checksum des fichiers
	* Lancement via cron
* Autid
	* Anaylse des syscalls, des accès fichiers
	* auditctl -w /etc/passwd -p war -k monitor-passwd
	* ausearch -ts today -k monitor-passwd
* Chiffrement avec Luks (cryptsetup)
	* plusieurs clefs possible pour un volume
	* pvcreate, vgcreate, lvcreate
	* luksFormat, luksOpen
	* /etc/crypttab, /etc/fstab

## Gestion avancée des contrôles d'accès

* authentification (mdp, clef, etc.)
* autorisation, droit d'accès
* traçabilité, qui fait quoi
* MAC, Mandatory Access Control
	* l'application ne fait que les tâches autorisées
* Linux Security Module
	* SELinux, AppArmor, TOMOYO, SMACK
	* DAC, MAC, RBAC, FBAC
	* GRSecurity, Systrace, FBAC-LSM, RSBAC
* systemd, remplace sysvinit
	* restriction d'accès réseau
	* file system en read-only ou invisible
	* gestion des capabilities
	* /tmp propre au service
	* systemctl, systemd-analyze
	* systemctl show --all httpd
	* slices, scope, services
* sudo
	* attribution de droits user/groups
	* (recodage de sudo en rust by boogy)
	* permet d'usurper l'identité d'un utilisateur
	* /etc/sudoers, /etc/sudoers.d/*
		* éviter les * dans les règles
	* sudo -s (shell), -i (interactif), -g/u (group/user)
	* sudo -e, sudoedit
		* ne pas laisser un utilisateur exécuté vim en root, !bash
		* ne jamais autorisé des interpréteurs ou éditeurs avec sudo
	* cat file | sudo tee [-a] file
	* NOPASSWD: ne demande pas le password à l'utilisateur
	* NOEXEC: empêche !bash dans vi ou less...
	* Règles
		* entre parenthèses (root:root) au lieu de (ALL)

## cgroups

* permet de limiter, policer et comptabiliser les ressources d'un ensemble de processus
* similaire à nice/renice, limits.conf mais plus flexible
* sont organisés hiérarchiquement, avec héritage
* plusieurs hiérarchies possibles
* filesys cgroup?, cgmanager, cgcreate, cgexec, cgclassify
* utilisé par d'autres systèmes tels que LXC, systemd
* un sous-système (ou resource controller) agit sur un groupe de tâches, e.g. un groupe de processus
* différentes règles s'appliquent :
	* une hiérarchie peut avoir un ou plusieurs sous-systèmes
	* etc.

## namespaces

* cgroups : mécanisme de métriques et mesures
* namespaces : mécanisme de gestion de la visibilité
	* un namespace limite ce que les processus à l'intérieur de ce namespace sont capables de voir.
	* pid, net, mnt, uts, ipc, user
		* mnt : permet d'attacher un processus à son propre système de fichier (un peu comme chroot)
