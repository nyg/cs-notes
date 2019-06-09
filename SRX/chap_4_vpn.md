# VPN

## Types de VPN

* **Site-to-site** : des bureaux délocalisés peuvent établir des connexions sécurisées entre eux sur un réseau public.
* **Remote access** : des utilisateurs individuels peuvent établir une connexion sécurisée avec un réseau informatique distant.

## Tunneling et Encapsulation

* **Tunneling** : protocole qui permet le mouvement sécurisé de données d'un réseau vers un autre, en utilisant l'encapsulation.
* **Encapsulation** : processus permettant à des paquets contenant des données privées de passer inaperçus sur un réseau publique.

## Technologies

### VPN avec TLS

* Traversent facilement les firewalls et NATs.
* Peuvent s'exécuter en user space.
* Compatibles avec beaucoup de plateformes.

#### Sans client (WebVPN)

* Ressources limitées par le serveur proposant le service VPN.
* Protège uniquement la connexion entre le navigateur et le serveur.

#### Client léger

* Client léger, e.g. application Java.
* Compatible avec un nombre restreint d'applications.
* Utilise TLS pour communiquer avec le serveur.

#### Full-tunnel

* Client léger (Web) ou client VPN
* Driver intercepteur de paquets en kernel space (droits admin).
* Compatible avec toutes les applications IP.
* Utilise TLS pour communiquer avec le serveur.

#### OpenVPN

* Utilise TLS pour l'authentification et la dérivation des clefs.
* Autre tunnel pour la protection des données (pas vraiment un tunnel TLS).
* Fonctionnement :
    * Une interface supplémentaire est créée (e.g. tun0),
    * Les applications envoient leurs données vers cette interface,
    * L'interface envoie ces données vers OpenVPN,
    * OpenVPN chiffre les données et les envoie sur l'interface, e.g. wlan0.
* Le payload d'un paquet TCP/UDP d'OpenVPN contient la trame chiffrée (AES-256) de la couche liaison encapsulée, avec un contrôle d'intégrité (SHA512).
* Obtentions des clefs :
    * Static Key Method :
        * clef partagée préprogrammée dans le client et le serveur
        * configuration simple
        * pas très fiable car réutilisation des clefs
    * TLS Method :
        * génération pseudo-aléatoire unilatérale des clefs ou
        * dérivation par échange de messages client-serveur.

### VPN avec IPSec

* Fourni des fonctionnalités de chiffrement et d'authentification du trafic IP.
* Protection sur les réseaux locaux et sur Internet.
* Mitigation des faiblesses d'IPv4 (écoute, modification, usurpation, DoS, MiTM).
    * Encapsulating Security Payload chiffre le payload des paquets IP.
    * Utilisation de checksum pour assurer l'intégrité des données.
    * Vérification des identités sans les exposer à un attaquant.
    * Permet le filtrage de paquets IP.
    * Authentification mutuelle avec clefs cryptographiques.

#### Modes d'encapsulation

* Mode transport :
    * entête IPSec rajoutée entre celle d'IP et de TCP,
    * paquet TCP chiffré et authentifié,
    * support d'IPSec nécessaire des deux côtés,
    * sécurité bout-en-bout entre deux hôtes.
* Mode tunnel :
    * nouvelle entêtes IP et IPSec rajoutées avant celle d'IP,
    * paquet IP chiffré et authentifié,
    * sécurité passerelle-à-passerelle (gw-to-gw),
    * trafic interne non protégé,
    * support d'IPSec au niveau des passerelles,
    * modes de déploiement : host-to-host, h-to-gw, gw-to-gw.

#### Vocabulaire IPSec

* **Security Association** (SA)
    * une SA pour envoyer et une autre pour recevoir
    * nécessaire pour la communication entre hôtes
    * type de protection, algorithmes, clefs, etc.
    * définie par : SPI, adresse IP de destination, identificateur de protocoles (AH, ESP)
    * stockée dans une db de SAs
    * determine le processing IPSec pour l'envoi et le décodage pour la réception
* **Security Parameters Index** (SPI)
    * permet au récepteur de choisir la bonne SA
    * contenue dans le paquet

* **Security Association Database** (SAD)
    * contient les paramètres pour chaque SA (durée de vie, infos AH & ESP, mode transport ou tunnel)
    * possédée par chaque hôte ou passerelle
* **Security Policy Database** (SPD)
    * défini quel trafic doit être protégé
        * Discard : ne pas laisser entrer ou sortir
        * Bypass : ne pas appliquer/attendre IPSec
        * Protect : applique/vérifie une sécurité (pointe vers une SA)
            * sortant : si la SA n'existe pas, en générer une avec IKE
            * entrant : écarter le paquet
    * possédée par chaque hôte ou passerelle

* **Authentication Header** (AH)
    * vérifie l'authenticité et l'intégrité d'un paquet, pas de confidentialité
    * mode transport : données + parties de l'entête IP
    * mode tunnel : paquet original + parties de la nouvelle entête IP
    * protection contre les attaques replay
* **Encapsulation Security Payload** (ESP)
    * ESP header + ESP trailer
    * mode transport : chiffre et authentifie les données, authentification optionnelle d'ESP
    * mode tunnel : paquet entier chiffré et authentifié, authentification optionnelle d'ESP
* **Internet Key Exchange** (IKE)
    * échange et négociation des politiques de sécurité
    * établissement des SAs (ISAKMP)
    * échange et gestion de clefs (Oakley)
    * Phase 1
        * négociation et établissement d'un canal sécurisé auxiliaire (utilisé pour l'authentification et la phase 2)
        * authentification de la source
        * confidentialité et intégrité des données
        * replay protection
        * mode principal (6 messages) et rapide (3 messages)
    * Phase 2
        * négociation des paramètres pour les SAs (ESP, AH, algos)
        * établissement de canaux sécurisés pour la transmission de données
    * DH utilisé pour l'échange de clefs
