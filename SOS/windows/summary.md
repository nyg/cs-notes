# Sécurité Windows

**Winlogon** permet la coordination de l'authentification (qui est effectuée par LSASS) ainsi que la création et la gestion de session.

**LSASS** permet d'authentifier les utilisateurs et émet les Access Token après une authentification réussie.

L'**Access Token** est délivré à l'utilisateur qui s'est authentifié. Il a une portée local, il est utilisé par Windows pour effectuer des descisions relative au contrôle d'accès, il est transmis aux processus et threads enfants. Il contient le SID de l'utilisateur, son groupe, ses droits et ses privilèges.

![winlogon](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/winlogon.png)

**LSASS** repose sur l'utilisation de paquetages d'authentification (qui sont des DLL) aussi appelés Security Support Packages. Les plus communs sont :

* **MSV** pour le support de l'authentification legacy utilisant LM/NTLM.
* **Kerberos** pour l'authentification Kerberos.
* **TsPkg** pour l'authentification RDP (Remote Desktop Sharing).
* **WDigest** pour l'authentification en SSO avec HTTP. Le mot de passe est exposé en clair. Possible de le désactiver depuis 2014 (clef de registre UseLogonCredential).

**EternalBlue** est une vulnérabilité critique du protocole SMB v1.0 permettant l'exécution de code arbitraire. MS17-010 est le nom du correctif de sécurité correspondant (mars 2017).

Le **Security Reference Monitor** (SRM) vérifie les autorisations suite aux requêtes de l'Object Manager (?). Le SRM ne vérifie que les droits d'accès. Il compare l'Access Token à l'Access Control List (Security Descriptor) (?) et à l'Integrity Level (?).

![srm](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/srm.png)

Les **Logon Types** indiquent comment se déroule l'authentification. Le mot de passe n'est pas toujours conservé en mémoire (LSASS), c'est le cas du Logon Type 3 (network). Les Logon Types communs sont :

* Interactive (2) : login depuis le clavier.
* Network (3) : login depuis une autre machine sur le réseau.
* Unlock (7) : déverouillage d'une machine.
* Remote Interactive (10) : login via remote desktop.
* Cached Interactive (11) : login depuis le clavier (MSCACHE).

## LM Hash & NTLM Hash

**LAN Manager Hash** (LM Hash) est un algorithme de hashage de mots de passe très faible (bruteforcable en quelques heures). Ses principales faiblesses sont :

* Caractères convertis en majuscules.
* Alphabet de 95 caractères.
* Longueur maximale de 14 caractères. Si inférieur, complété par des caractères nuls.
* Le résultat est la concaténation du hash des 7 premiers caractères avec celui des 7 suivants :
    * beaucoup plus facile à bruteforcer (2 * 95 ^ 7 vs 95 ^ 14),
    * permet d'identifier les mot de passe de moins de 7 caractères (car la deuxième partie est constante).

### Pass-the-hash

Remplacé par le NT LAN Manager Hash (NTLM Hash) qui est aussi très faible. Est aussi vulnérable à l'attaque Pass-the-hash rendant inutile le bruteforçage du hash. La connaissance du hash NTLM est suffisante pour produire la réponse challenge demandée par le serveur. Ne peut pas être desactivé pour des raisons de compatibilité.

![pass-the-hash](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/pass-the-hash.png)

## Active Directory

Base de données qui centralise toutes les informations relatives au domaine. 3 niveaux hiérarchiques : domain, tree (collection de domaines), forest (collection de trees). Différents types d'objets : Users, computers, groups, organizational units.

## Group Object Policy (GPO)

Permet la centralisation de la configuration des devices. Stocké sur le share SYSVOL dans des fichiers XML. Appliqués automatiquement au démarrage de la machine.

## Mitigations

* KPP : détecte la modification de structures critiques.
* DEP : empêche du code d'être exécuté dans des sections RW.
* ALSR : rend aléatoire certaines adresses mémoire.
* SMEP : restreint l'exécution entre l'espace kernel et user.
* CFG : force les sauts indirects à être effectués vers des fonctions valides.

* KPP et SMEP appliqués sur tout le système.
* DEP, ASLR and CFG appliqués par processus.

## Exploit Guard

Prévention des intrusions : exploit protection, attack surface reduction rules, network protection, controlled folder access, process isolation.

## Mimikatz

* Debug system processes (SeDebugPrivilege)
    * permet à Mimikatz de récupérer les mots de passe stockés en mémoire
* Shutdown the computer (SeShutdownPrivilege)
* Change date and time (SeSystemtimePrivilege)
* Use another access token (SeImpersonatePrivilege)
    * permet de récupérer des Access Token (utilisé avant l'apparition de Mimikatz).

**Mimikatz** est un outil développé par gentilkiwi et qui permet d'extraire les mots de passe (en clair et hashé), les codes PIN et les tickets Kerberos stockés en clair dans la mémoire. Mimikatz peut aussi exécuter une attaque pass-the-hash et pass-the-ticket ainsi que créer un Golden tickets.

## Isolation des processus

Différents méchanismes permettent l'isolation des processus :

* **Mandatory Integrity Controls** (MIC) : isole les processus d'un même utilisateur. Limite l'accès en écriture. Basé sur les Integrity Levels. Existe depuis Vista.
* **AppContainer** : permet le sandboxing d'une application, étend les Integrity Levels pour pouvoir isoler une application du matériel, des fichiers, du registre, etc. Permissions spéciales avec les Capability. Peut être activé avec le flag de compilation /APPCONTAINER ou depuis les paramètres d'une application. Existe depuis Windows 8.
* **Application Guard** : isole Edge en utilisant la virtualisation. Existe depuis Windows 10. Un exploit kernel n'est plus suffisant pour bypass.
* **Windows Sandbox** : Application Guard mais pour d'autres application qu'Edge.

Différents niveau des **Integrity Levels** existants : Untrusted, Low, Medium, High & System.

## Metasploit

À l'origine, **psexec** est un outil pour exécuter des commandes sur une machine Windows à distance. Utilise SMB pour mettre un binaire dans le share ADMIN$ puis crée un remote service qui l'exécute. Nécessite des identifiants valides.

Intégré comme module dans Metasploit et permet de déployer un shell Meterpreter en utilisant reverse_tcp ou bind_tcp comme payload.

![psexec](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/psexec.png)

Dans Metasploit, un payload correspond à un exploit module. Les 3 types principaux sont :

* Single : payload de taille limitée contenu dans un seul module.
* Stager : payload qui initie une connexion entre l'attaquant et la victime dans le but de télécharger un stage, de l'injecter en mémoire et de le faire exécuter. De petite taille et fiable.
* Stage : payload téléchargé par un stager. Permet d'avoir des fonctionnalités avancées sans limite de taille, e.g. Meterpreter.

![staging](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/staging.png)

Modules pour l'extraction des motss de passe : hashdump, cachedump, lsa_secrets, gpp, domain_hashdump.

## Kerberos

Kerberos est un protocole d'authentification intégré à Windows afin de remplacé NTLM. 3 composant principaux :

* Clients
* Services
* Authentication Server : une tierce partie de confiance responsable de vérifier l'identité du client et de délivrer les clefs de session aux participants.

### Tickets

* Un **Ticket-Granting Ticket** (TGT) est le premier ticket que peut obtenir un client. Son seul but est de permettre au client d'obtenir d'autres tickets (e.g. des Service Tickets). Kerberos ne garde pas la trace des TGT qu'il délivre. Le TGT délivré est chiffré avec le hash du mot de passe du compte krbtgt. Il n'est pas censé être déchiffré par le client ou par le service.
* Un **Service Ticket** délivré par le contrôleur de domaine est chiffré avec le hash du service.

* **Silver Ticket** : en connaissant le mot de passe du service on peut forger un Service Ticket. On peut alors impersonner n'importe quel utilisateur du service.
* **Golden Ticket** : en volant le compte krbtgt il est possible de forger n'importe quel ticket, même pour des utilisateurs inexistants.

Les clients et les services partagent un secret avec l'Authentication Server dérivé du mot de passe du compte. Ce secret n'est pas transmis sur le réseau.

![kerberos](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/kerberos.png)

### Pass-the-ticket

Les TGT sont gardés dans la mémoire cache et sont valides pendant 10 heures. Ils peuvent être extraits et importés dans d'autres sessions ou systèmes à des fins d'authentification. Attaque possible avec Mimikatz.

### Kerberoast

Dès qu'un Service Ticket a été obtenu, il est possible de bruteforcer (offline) le mot de passe du compte associé au service.

Il y a deux types de SPN dans l'Active Directory :

* **Host-based** : créé quand la machine rejoint le domaine avec un mot de passe très long et complexe.
* **Arbitrary** : peut être n'importe quel compte de domaine lié à un service. Mot de passe défini par un humain et donc faible et/ou rarement changé.

L'attaque ne s'effectue donc que sur les Arbitrary SPN. Elle peut être mitigée par l'utilisation d'un mot de passe complexe.

![kerberoast](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/kerberoast.png)

## MSCACHE

MSCACHE garde la trace des 10 derniers comptes domaine s'étant connectés sur la machine. Ces utilisateurs peuvent se connecter quand le contrôleur de domaine n'est pas disponible. Mots de passe hashés avec DCCH.

## SAM

La SAM est une base de données sauvegardant les détails des comptes locaux (y compris les mots de passe, hashés avec LM et NTLM hash). La base de données est chiffrée avec la SYSKEY et n'est pas accessible en lecture pendant que l'OS tourne (sauf par le compte SYSTEM).

## Credential Manager

Password manager pour Windows. Mots de passe chiffrés dans des vaults, utilise la DPAPI (Data Protection API). Deux parties : web & windows credentials.

## Group Policy Preferences (GPP)

GPO qui permet de déployer des mots de passe sur des machines en remote. Mots de passe chiffrés avec une clef commune à toutes les installations Windows (cPassword). Stocké sur le share SYSVOL, lisible par n'importe quel domain user. Déprécié par Windows.

## NTDS

La base de données NTDS (stockée dans le fichier NTDS.dit) contient toutes les informations relatives au domaine (y compris les mots de passe, hashés avec LM et NTLM hash). Elle est aussi chiffrée avec la SYSKEY. Elle peut être répartie sur plusieurs serveurs. Elle offre principalement l'authentification et les directory service (NETLOGIN et SYSVOL). Aussi protégée contre la lecture mais la restriction peut être bypassée en créant un Volume Shadow Copy en utilisant `vssadmin`.

## SYSKEY

Clef utilisée à différents endroits. Clef calculée en utilisant 4 DWORD obfusqués. Facilement récupérable.

## DPAPI

API permettant de chiffrer différentes données. Utilise une clef dérivée du mot de passe de l'utilisateur.

## LAPS

LAPS (Local Administration Password Solution) permet de configurer un mot de passe local pour les membres du domaine. Les mots de passe sont changés régulièrement. Stockés dans l'Active Directory et protégés par une ACL.

## Niveau de privilèges pour les services

* LocalService (Low, Anonymous auth.)
* NetworkService (Low, Account auth.)
* LocalSystem (Full, Account auth.)

## Credential Guard

Technologie permettant d'éviter le vol de mots de passe, de hashes et de tickets. Utilise VBS (Virtualization-based Security). VBS stock les données sensibles dans un processus LSA (Local Security Authority) isolé. Activation via GPO. Bonne protection mais impact conséquent sur les performances et est incompatible avec certaines fonctionnalités de Windows.

![credential-guard](https://github.com/nyg/cs-notes/raw/master/SOS/windows/img/credential-guard.png)

## Smart Cards

Remplace le mot de passe par une clef privée. L'utilisateur ne connait pas sa clef privée et ne peut donc pas la donner. PIN requis (2FA). Repose uniquement sur Kerberos. LSASS n'a pas accès au mot de passe et ne peut donc pas calculer le hash NTLM pour l'authentification. Mais le hash NTLM de l'utilisateur est retourné dans réponse de pré-authentification (compatibilité). N'empêche donc pas les attaques *pass-the-hash*.

### Virtual Smart Cards

Support ajouté pour les VSC depuis Windows 10. Se base sur le TPM (Trusted Platform Module – cryptoprocesseur sécurisé). Non-exportability, isolated cryptography, anti-hammering. Base pour Windows Hello for Business (pin + biométrique).

## LSA Secret

LSA Secret est une clef de registre située dans la base de données SECURITY. Différents types d'identifiants y sont stockés (services, auto-login, VPN). Les mots de passe sont chiffrés.

## LSA Protection

Augmente la sécurité du processus LSASS avec l'utilisation de la technologie Protected Process (empêche les accès mémoire + debug). Disponibilité Windows 8.1+. Peut être bypassé en utilisant un driver kernel, e.g. mimidrv.sys.

## DMA Attack

Une attaque DMA (Direct Memory Access) permet à un device d'accéder directement à la mémoire physique de la machine (transfert rapide et faible utilisation du CPU), de modifier les méthodes de vérification de mot de passe et de démarrer des nouveaux processus. Possible via une interface PCI ou PCI-express. Depuis 2008. Protection depuis Windows 10 (bloque les devices quand l'ordinateur est verrouillé + Kernel DMA Protection).

## Skeleton Key

Toutes les authentification d'un domaine sont effectuées par LSASS. Avec un accès SYSTEM au contrôleur de domaine, une backdoor peut être injectée. Une skeleton key est un mot de passe qui est valable pour chaque compte du domaine.

## Protected Users Group

Retreint ce que les membres de l'Active Directory peuvent faire dans le domaine. Évite que leur compte soit utilisé pour des services non-autorisés. Authentification via Kerberos.

## Bitlocker

Full Disk Encryption de Windows. Vista+. Utilise la VMK (Volume Management Key). Clef stockée dans le TPM (SRK/PCR). Ajout d'un PIN possible.

## Secure Boot

Protocole UEFI. Vérifie la signature digitale des firmware, du bootloader et des drivers. Protège contre l'attaque EvilMaid.

## Cold Boot Attack

Refroidir la machine pour alonger le temps de rétention des données de la DRAM. Rend Bitlocker vulnérable si l'hibernation n'est pas activée (car la VMK est dans la mémoire).
