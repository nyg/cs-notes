# Firewalls

* Point d'étranglement pour le contrôle et la surveillance du trafic réseau.
* Interconnecte des réseaux dont les niveaux de confiance sont différents.
* Impose des restrictions sur des services réseau.
* Idéalement à l'abri des intrusions.

## Firewall « appareil »

* Appareil dont la seule fonction est d'être un firewall.
* Matériel dédié, puissant mais cher.

## Firewall « réseau »

* Sur un routeur ou switch : protège un réseau entier.
* Sur un ordinateur ou serveur : flexible et peu cher mais plus lent

## Fonctionnement

* Utilise des informations de la couche transport :
    * adresses IP et ports, source et destination
    * protocol, e.g. TCP, UDP, ICMP
    * flags TCP, e.g. SYN, ACK, RST
    * type de message ICMP
* Certains firewall peuvent aussi analyser la couche application

## Règles de filtrage

* Autorisation par défaut
* Interdiction par défaut
    * penser à autoriser les réponses
    * et à utiliser des règles statefull
* Accès conditionné

## Firewall statefull

* Garde une trace de chaque paquet sortant afin de le faire correspondre avec un ou plusieurs paquets entrant.
* Fonctionne aussi avec les protocol stateless comme UDP.

## Architectures possibles

### Firewall personnel

* Tourne en local, sur la machine.
* Bon marché et simple.

### Firewall intégré au routeur

* Tourne sur le routeur.
* Tout trafic passe par le routeur, et donc par le firewall.
* Un serveur peut compromettre tout le réseau.

### Firewall standalone

* Tourne sur sa propre machine.
* Placé après le routeur.
* Surface d'attaque plus petite.

### Zone démilitarisée (DMZ)

* Isolée du réseau privé interne.
* Contient des services devant être plus exposés à Internet pour fonctionner.
* Le firewall se trouve à l'intersection entre le routeur, le LAN et la DMZ.

### Double firewall (sandwich)

* Deux firewalls sont utilisés, un après le routeur et avant la DMZ, l'autre après la DMZ et avant le LAN.

### Firewall application (proxy)

* Route le trafic d'un port à l'autre.
* Comprend le trafic des protocol applicatifs (HTTP, etc.).
* Inconvénient : il faut écrire un proxy pour chaque service, performances moindres.
* Avantage : bonne sécurité, conscience de la couche application.

## En pratique : netfilter et iptables

* netfilter : framework du kernel Linux, permet des opérations réseau au niveau des paquets.
* iptables : outils de configuration pour netfilter (fonctionnalités NAT et firewall)

### iptables

* Contient des tables (e.g. filter, nat, mangle).
* Ces tables contiennent des chaînes (e.g. INPUT, OUTPUT, FORWARD).
* Chaque chaîne contient une politique ainsi que des règles.
* Chaque règle contient un critère et une cible. Si le critère est respecté la cible est exécutée. Si un paquet ne correspond à aucune règle, la politique de la chaîne est exécutée.
* Exemples de critères : protocole, adressse et port (source et destination)
* Exemples de cibles/politique : ACCEPT, DROP, REJECT

```sh
# Acceptation des paquet sortant du firewall à destination du port 53.
$ iptables -t filter -A OUTPUT -p udp --dport 53 -j ACCEPT
```

#### Module conntrack

* Permet d'avoir des règles statefull. Activation avec l'option `-m state` ou `-m conntrack`.
* Utilisation avec l'option `--ctstate` avec les valeurs suivantes :
    * NEW : premier paquet d'une connexion
    * ESTABLISHED : paquet d'une connexion en cours
    * RELATED : paquet apparenté à une autre connexion (e.g. FTP qui utilise deux ports différents)
    * INVALID : paquet sans état approprié

```sh
$ iptables -t filter -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
$ iptables -t filter -A INPUT -m conntrack --ctstate INVALID -j DROP
```
