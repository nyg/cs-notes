# Résumé AIT

## Sauvegarde

* Recovery Point Objective (RPO) ou **perte de données maximale admissible** : la quantité de données qu'un système IT peut être amené à perdre à la suite d'un incident.
* Recovery Time Objective (RTO) ou **durée maximale d'interruption admissible** : le temps maximale acceptable durant lequel le service IT peut être HS après un incident.
* Classification des sinitres :
  * niveau 1 : application ou serveur affecté,
  * niveau 2 : centre de données affecté,
  * niveau 3 : zone urbaine affectée.
* Raisons principales des pertes de données :
  * défaillance du matériel (44%),
  * erreur humaine (32%),
  * défaillance du logiciel (14%),
  * virus (7%),
  * catastrophe naturelle (3%).

* **Archivage** : peut être nécessaire lorsque certain documents (e.g. légaux) doivent être conservés plusieurs années ou même décennies.
* Critères d'une solution de sauvegarde :
  * le RPO et RTO doivent être garantis,
  * les copies de sauvegarde doivent être physiquement éloignées du système IT,
  * au moins une copie doit pouvoir être récupérée avec une haute probabilité,
  * dans le cas d'une erreur utilisateur, doit permettre de restaurer sans perdre trop de modifications.

* Règle de sauvegarde en trois points (3-2-1, par Scott Hanselman) :
  * **3** copies de données doivent exister,
  * **2** supports différents doivent être utilisés,
  * **1** des copies doit être distante.
* Chaque fichier possède des métadonnées mais elles ne sont pas toutes sauvegardées par les différents programmes de sauvegarde. Au minimum :
  * nom et chemin,
  * taille,
  * date de création,
  * date de dernière modification,
  * date de dernier accès.
* Les fichiers dans `/dev` sont créés à partir des règles dans `/etc/udev/rules.d`.

    ```sh
    stat, mdls (macOS) # Métadonnées de fichiers
    mount, umount      # Montage de systèmes de fichiers
    mkfs               # Création d'un système de fichiers
    df                 # Informations sur un système de fichiers
    fsck               # Vérification d'un système de fichiers

    # Création d'une table de partition
    fdisk, sfdisk (scriptable)
    parted, gparted (gui)
    ```

* Niveaux de sauvegarde :
  * niveau 0 : sauvegarde complète,
  * niveau 1 : sauvegarde différentielle (par rapport au niveau 0),
  * niveaux 2–9 : sauvegarde ce qui a changé depuis la dernière sauvegarde de niveau inférieure.

```sh
tar -c create
    -x extract
    -t list content of archive
    -f archive file
    -z gzip
    -j bzip2
```

* Sauvegarde **bare-metal** : sauvegarde un serveur afin de le restaurer sur un nouveau qui n'a aucun logiciel pré-installé.

* **Near Continuous Data Protection** (NCDP) : sauvegarde de fichier à quasiment chaque modification (en vrai chaque minute) au lieu d'une fois par jour.
* Avantages du **stockage virtuel** :
  * système de fichiers à grande capacité à partir de plusieurs disques physique,
  * changement dynamique de la taille d'un disque virtuel,
  * gestion des disques plus flexible,
  * partage d'un pool de disques physiquess entre plusieurs serveurs.

* Snapshot ou **instantané** d'un disque virtuel :
  * copie complète du disque virtuel, la copie est quasiment instantanée car il suffit de copier l'index contenant les pointeurs,
  * après la copie, les écritures sont traitées de manière spéciales : copy-on-write (ou redirect-on-write), les blocs non modifiés sont partagés entre les disques virtuels et les modifications sur un disque n'affecte pas les autres disques.
  * Les instantanés sont généralement limité à de la lecture seule contrairement au disque dit **actif**.
  * Exemples : LVM, LDM (Windows), SAN, logiciels de virtualisation.
* Les snapshots existent aussi au niveau du système de fichiers (e.g. Time Machine).

* Synchronisation de fichiers (e.g. iCould, Dropbox, rsync) :
  * synchronise le contenu de deux ou plus endroits de stockage,
  * utilisé quand le contenu évolue lentement,
  * peut être locale ou distante, unidirectionnelle ou bidirectionnelle,
  * peut se baser sur un historique de fichiers ou pas.

* La sauvegarde de bases de données est plus compliquée :
  * plus complexes qu'un système de fichiers,
  * ne peuvent être arrêtées,
  * restauration nécessaire à un instant précis (point-in-time recovery),
  * plusieurs éléments : OS, SGBDR, schéma, données,
  * sauvegarde physique ou logique,
  * un  checkpoint est un instant ou aucune donnée n'est dans la mémoire (toutes sont sur disque)

## Les liens

* Un lien symbolique peut (contrairement à lien physique) :
  * pointer vers un fichier d'un autre système de fichiers,
  * pointer vers un répertoire,
  * être distingué du fichier original,
  * pointer vers un autre lien.
* Avec un lien physique, déplacer le fichier original ne casse pas le lien (contrairement à un lien symbolique).
* Un lien symbolique fonctionne toujours si le fichier original est supprimé puis recréé.

## High performance systems

* Time limits:
  * 0.1 second: system is reacting instantaneously,
  * 1 second: limit not to interrupt the user's flow of thought,
  * 10 seconds: limit to keep the user's attention.
* A system is **scalable** if it can handle a growing amount of work or can be enlarged to accomodate that growth.
  * Vertical scaling (*scale-up*): faster CPU, more memory, etc.
  * Horizontal scaling (*scale-out*): more computers working in parallel.

* A **proxy** sits on the client-side. A **forward-proxy** is deployed by a website to forward client requests to different servers.
* A **load-balancer** is a reverse-proxy that distributes requests evenly across multiple servers. Can be layer 4 (TCP) or 7 (HTTP).
* Round-robin DNS is a load-balancing mechanism. The DNS server responds with multiple A records (mulitple IPs for the same domain name) changing the order for each request, assuming the client will use the first one.
  * Does not know about server load, difficult to add servers.

* Load-balancers:
  * with hardware: professionnal solutions,
  * with software:
    * IP Virtual Server: e.g. Linux Virtual Server (layer 3-4 load-balancer),
      * application-layer: e.g. HAProxy, Poung, Apache.

* *Staleness* problem: sending load information from server to load-balancer is slow.
* The server can specify the **Cache-Control** header:
  * `max-age`: maximum time in seconds the resource can be used,
  * `no-cache`: the resource can be reused but must first check with server,
  * `no-store`: the resource must not be reused.
  * `public`: the resource can be used by multiple users,
  * `private`: the resource is intended for a single user.
* A separate mechanism is the **ETag** header:
  * the server computes a token, usually a fingerprint of the file and includes it in the `ETag` header of the response,
  * when the client sends a requests for the file it includes the `If-None-Match` header with the value of the token,
  * the server either responds with the new file or with 304 Not Modified.
  * This only happens if the `max-age` value has expired.
* If the resource is not supposed to change, set a value of 365 days for `max-age` and use a fingerprint in the resource's name so it can be invalidated by the server if necessary (by issuing a new name).
* **Content Delivery Network** (CDN):
  * *Origin* server: the web server operated by the CDN's client,
  * *Edge* server: the CDN's web cache.
* Caching architectures:
  * **look-aside**: the cache always answers to the application,
  * **look-through**: the cache performs requests to the database if needed and then sends the response to the application.

## Virtualization

* Emulation: process of implementing the interface and functionality of a system on a system with a different interface and functionality (e.g. ARM on x64, game consoles)
* Hardware virtualization:
  * **Type 1**: bare metal hypervisor, e.g. VMware ESXi,
  * **Type 2**: hosted hypervisor, e.g. VirtualBox, VMware Workstation.
* To support the VM's architecture, a software called the **virtual machine monitor** is used (VMM).
* Properties of hardware virtualization:
  * **binary compatibility**: the guest behaves like a real machine to the guest OS,
  * **interposition**: all guest actions can be inspected, denied, etc.
  * **isolation**:
    * data in one VM cannot be accessed from another one,
    * a VM with a high load cannot impact the performance of another one
  * **encapsulation**: the VM state can be written to file.
* Usage:
  * server virtualization in companies (e.g. VMware ESXi, Citrix XenServer),
  * cloud computing (e.g. AWS, Azure)
  * development VMs (e.g. VirtualBox, Parallels Desktop),
  * desktop virtualization.
* Companies use virtualization to:
  * have multiple secure environments,
  * have failure isolation,
  * support a mixed-OS environment,
  * have a better system utilization,
  * reduce energy costs.
* Three hardware virtualization techniques:
  * Full virtualization:
    * guest is not aware it's on a virtualized platform,
    * best isolation and security,
    * lower performance.
    * For privileged instructions: trap them and virtualize their execution, emulate the behavior of those who can't be, user-level code runs on host CPU.
  * Para virtualization:
    * guest OS is modified and cooperates with the hypervisor (is usually open-source),
    * less binary translation, room for optimization so better performances.
  * Hardware-assisted virtualization:
    * host CPU provides special instructions: new root mode below ring 0, virtualized MMU, etc.
    * no need for binary translation or para-virtualized OSes,
    * best performances.
    * Introduced in 2006: Intel VT-x, AMD-V.
* **QEMU** (Quick Emulator): generic open-source emulator and virtualizer.
* **Kernel Virtual Machine** (KVM): Linux feature allowing it to act as a type 1 hypervisor.
* Virtual networks:
  * NAT is the default mode, each VM think it's in its own isolated network, each VM has the same IP.
  * Bridge mode: the guest NIC is bridge with that of the host's. It has access to the same network as the host.
  * Internal network: all VMs are connected to an isolated network. They can talk to each other.
  * Host-only adapter: like internal network but the host is also part of that network.
  * Internet sharing (NAT, the real default): like host-only adapter but with internet access for the VMs.

* **Vagrant** creates and configures virtual development environment (works on top of VirtualBox and VMware). Offers a repository of pre-configured VMs (aka "boxes").
