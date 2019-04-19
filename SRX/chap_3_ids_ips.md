# IDS & IPS

* Intrusion Detection and Protection Systems

## Intrusion

* Répétition d'un comportement inhabituel.
* Exploitation de vunérabilités connues.
* Séquences de paquets ou routes incohérentes.
* Contenu de trafic suspect.

## IDS

* L'objectif d'un IDS est d'alerter qu'une attaque a eu lieu sur un réseau et cela le plus rapidement possible (ordre de la minute) avec le plus de précision possible (éviter les faux positifs).
* L'IDS peut se voir obliger de patienter avant de déclencher l'alerte, certains scans peuvent être effectués très lentement (plusieurs mois).
* Le canal de notification doit être protégé, il faut éviter qu'un attaquant puisse empêcher la notification.
* Idéalement, catégorise et identifie l'attaque. S'adapte aux nouvelles attaques.
* Peut émettre des recommandations :
    * évaluation de la vulnérabilité de la cible
    * informations sur les corrections à appliquer
* Identifie tant les attaques internes qu'externes.
* N'est pas firewall, ne protège pas contre les attaques en cours.
* La récolte de données par l'IDS peut poser des problèmes juridiques.
* Peut être placé avant le firewall afin d'observer des attaques externes et/ou après le firewall afin d'observer des attaques internes.

### Signature

* Séquence d'octets qui caractérise le paquet d'intrusion de manière sûre. Permet de reconnaître une intrusion ou attaque.

### HIDS

* Logiciel installé sur une machine hôte.
* Surveille uniquement le trafic destiné à la machine hôte.
* A accès à certains fichiers système critique (e.g. /etc/passwd).
* Peut surveiller le trafic chiffré (avantage principal).
* Consomme des ressources de la machine hôte.
* Données généralement envoyées vers un système central pour analyse.
* Si l'hôte est compromis, les données aussi.

### NIDS

* Surveille tout le trafic du réseau.
* Compare le trafic à une base de données de signatures d'attaques.
* Ne peut surveiller le trafic chiffré.
* Peut intercepter les trames ethernet.
* Pas d'impact sur les performances.
* Plus résistant aux manipulations.
* Indépendant des OS.
* Pertes de paquets possible.

## IPS

* Est une extension d'un IDS.
* Placé in-line afin de pouvoir bloquer les attaques.
* En plus d'émettre des alertes, peut écarter dess paquets malicieux et bloquer le trafic venant d'un attaquant.

### HIPS

* Peut agir sur les communications entrantes et sortantes.

### NIPS

* Peut bloquer le trafic, reconfigurer le firewall en temps réel.
