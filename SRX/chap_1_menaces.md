# Chapitre I : Menaces réseaux

## Menaces

* **Vulnérabilité** : faiblesse dans un logiciel ou système permettant à un adversaire de causer des dommages aux utilisateurs du logiciel/système.
* **Attaque**, **exploit** : techniques utilisées pour exploiter une vulnérabilité.
* Ressources sensibles → vulnérabilités → exploits → contre-mesures.

### Agent menaçant (threat agent)

* sans cible spécifique : virus, worms, trojans, etc.
* employés : staff, consultants, etc.
* organisations criminelles
* entreprises concurrentes
* erreur humaine
* activistes

### Vulnérabilités

* Sources : authentification, cryptography, erreur de logique, validation des entrées, etc.
* Cibles : navigateurs, back-up, phishing, etc.

## Tests d'intrusion

### Objectifs

* Escalation de privilèges, e.g. root.
* S'emparer d'un compte admin.
* Montrer au client ce qu'un attaquant peut faire.
* Collectionner des "trophées", e.g. mdp, docs confidentiels.
* Faire comprendre au client l'importance de la sécurité.

### Types

* **White box** : accès aux diagrammes de réseau, codes sources et autres informations utiles. Utilisé lors d'un budget et facteur temps limités. Scénario le moins réaliste.
* **Black box** : aucune information à part le nom de l'entreprise, voir une plage d'adresses IP. Manière fiable et réaliste de ce qu'un attaquant peut faire.
* **Gray box** : mélange des deux. Informations limitées. Méthode la plus efficace.

### Méthodologies

* Il n'existe pas une unique méthodologie. Cf. OWASP, OSSTMM, ISSAF. Les professionnels développent leurs propres méthodologies.

#### Planification (i.e. paperasse)

1. Prise de contact entre le client et l'équipe de pentest.
2. Définir la portée (scope, ce que l'on va faire et surtout ce que l'on ne va pas faire) et l'approche.
3. Se mettre d'accord sur les cas de test et chemins d'élévation de privilèges.
4. Signature de l'accord/contrat (protection légale, juridique). Définir l'équipe et les heures de travail.

#### Évaluation (i.e. pentest)

1. **Collecte d’informations** à partir de sources publiques techniques (DNS, WHOIS, etc.) et non-techniques (Facebook, Google, etc.).
2. Produire une **topologie du réseau**, hôtes vivants, scan de ports, identification des services et OS, détection de routeurs et firewall.
3. Identification, énumération et classement de **vulnérabilités** trouvées. Identification de chemins et scénarios d'attaques.
4. **Intrusion** : accès non autorisés, contournement des mesures de sécurité. Développement d'outils, de scripts et de *proof-of-concept*. Vérification ou rejet des vulnérabilités. Documentation.
5. **Obtention d’accès et élévation de privilèges** : découverte de usernames, passwords, etc. Obtention de droits d'admin et exploitation de vulnérabilités locales. Contre-mesures : IDS, anti-virus, màj.
6. **Énumérer encore plus** : répéter les étapes précédentes avec les privilèges gagnés.
7. **Compromettre les utilisateurs/sites distants** : compromettre d'autre services distants communiquant avec celui compromis.
8. **Conserver l’accès** : tunnel VPN, tunnel protocol, backdoors, rootkits, etc.
9. **Éffacer ses traces** : nettoyage de logs (pour montrer qu'un attaquant peut le faire).

#### Rapport

* **Verbale** : si une vulnérabilité critique est détectée, elle l'est annoncée tout de suite.
* **Écrit** : résumé, portée du projet, liste d'outils, d'exploits, dates des tests, résultats des tests effectués, vulnérabilités trouvées, recommandations et actions.

À la fin, effacer les traces du pentest…

## Découverte passive

* **Sniffing** : forme la plus basique de découverte passive. Utilise  des analyseurs de réseau (sniffers), e.g. Wireshark, ettercap, airodump-ng.
* Pour fonctionner, un sniffer doit être connecté à : un hub du réseau, un switch (ARP poisoning, MAC overflow).
* **Host discovery** : identifier passivement les hôtes, éviter la détection des IDS, réduire les perturbations. Exemples : messages broadcast (DHCP, ARP), Metasploit Passive Network Discovery.
* **Server DNS** mal configuré, découverte de services et serveurs. Difficilement détectable.
* **Banner Grabbing** : informations sur les logiciels et services.
* **Google Discovery** : fichiers, cartes de crédits, passwords, webcams, voip, etc.

## Découverte active

### OS Fingerprinting

Collection de paramètres de configuration TCP/IP qui permettent de conjecturer l'OS utilisé, les services utilisés et les ports ouverts.

### Port scanning

* **port ouvert** : l'application accepte activement les packets TCP/UDP.
* **port fermé** : aucune application n'écoute le port en question, mais l'OS répond qu'aucune connexion n'est acceptée.
* **port filtré** : état inconnu, le système ne retourne aucune information.

Port ouvert → service → application → vulnérabilité → exploit

* **TCP (connect)** : 3-way handshake, si la connection est établie alors le port est ouvert. Bruillant. Si le port est fermé on reçoit un RST, si filtré rien.
* **Idle** : scan TCP en utilisant une machine intermédiaire (zombie). Aucune interaction entre le scanneur et la cible. Très sophistiqué.
* **SYN** : Échange SYN ; SYN/ACK ; RST. Le 3-way handshake n'aboutit pas, la connexion n'est pas établie, mais si SYN/ACK est reçu alors le port est ouvert.
* **FIN, NULL & XMAS** : Envoi d'un packet TCP avec le flag FIN à 1 (FIN), aucun flag à 1 (NULL), les flags FIN, PSH et URG à 1 (XMAS). Si on reçoit un RST le port est fermé, sinon il est ouvert ou filtré.
* **UDP** : envoi d'un datagram UDP, s'il n'y a pas de réponse le port est ouvert ou filtré, si l'OS renvoie un *ICMP port unreachable* le port est fermé.
	
## Attaques actives

* Man-in-the-middle
* ARP spoofing/poisoning
* IP Spoofing (utilisé dans certaines attaques DoS)
* Intrusion (cheval de Troie, logiciel contrôlé à distance, clef USB, email, failles logiciel, etc.)

## Dénis de service

* Attaque des plus courantes.
* Peut faire tomber une connexion Internet, un système ou un réseau entier. Peut occasionner des pertes de données.
* Utilisation de **botnet** (machines sous le contrôle des attaquants), de réseaux de machines compromises (à louer). De l'ordre de quelques centaines de milliers de machines.
* **Smurf** : attaque DDoS en utilisant un ping ICMP avec comme adresse IP source celle de la cible. Les réponses ICMP inondent la cible.
* **SYN flood** : établissement de connexions TCP (3-way handshake) forcant le serveur à allouer beaucoup de ressources.