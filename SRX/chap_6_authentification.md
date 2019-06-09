# Authentification

## PKI

* Alice calcule un hash du certificat et de la clef publique reçus de Bob
* Alice utilise la clef publique du CA pour déchiffrer la signature
* Alice compare son hash avec la signature déchiffrée

* Alice envoie un challenge à Bob
* Bob chiffre le challenge avec sa clef privée et l'envoie à Alice
* Alice déchiffre le challenge reçu avec la clef publique de Bob
* Alice compare le challenge original avec celui qu'elle a reçu et déchiffré

## Kerberos

* Permet l'authentification sur un réseau non-sécurisé
* Orienté client-serveur
* Messages Kerberos protégés contre l'écoute clandestine et le replay
* Tiers de confiance nécessaire

* **Key Distribution Center** (KDC)
    * Authentication Server (AS)
    * Ticket Granting Server (TGS)
* **Ticket**
    * Ticket Granting Ticket (TGT)
    * Client to Server Ticket (CST)
* **Service Server**

(schéma page 39)

* Inconvénients
    * repose sur la disponibilité d'un serveur central
    * exigences strictes en matière de délais
    * toutes les authentification gérées par un KDC central

## RADIUS

* Utilise UDP
* Utilisé par les FAI
* Base de la sécurité WPA Enterprise

* Authentification (PAP) + Authorisation
* Accounting : journalise les ressources accédées par un utilisateur

* Seul le mot de passe est protégé, le reste est visible
* Protections additionnelles : IPSec, réseaux dédiés
