# Sécurité des réseaux sans fil

* 2 bandes : 2.4 et 5 GHz
* Chaque bande divisée en canaux
    * 2.4 GHz : canaux 1, 6 et 11

* Active scanning : on diffuse le nom du wifi auquel on veut se connecter
    * permet d'avoir l'historique des connexions d'un client
* Passive scanning : on se connecte à l'un des wifi dont le nom est diffusé

* Trame Beacon : contient le nom du réseau, le débit, la sécurité, etc.

* Écoute passive et analyse de trafic facile
    * WEP : clef nécessaire
    * WPA/2 : clef nécessaire + capture handshake
    * WPA2 Enterprise : impossible
* Idem pour l'injection de paquets
* Interception et suppression possible avec antennes directionnelles

* AP malicieux, détournement de session, DoS

## WEP

* Clef partagée
    * 40 ou 104 bits
    * connue par l'AP et les STAs
    * dérivée à partir d'un mot de passe
* IV de 24 bits
* RC4 pour le chiffrement
* Integrity Check Value (ICV), Cyclic Redundancy Check (CRC)
* Algorithme :
    * Calcul du ICV sur le corps de la trame
    * Ajout du ICV au corps de la trame
    * Enchaîner l'IV avec la clef partagée pour créer la seed RC4
    * Générer une séquence pseudo-aléatoire et XORer avec trame + ICV
* Utilisation de quelques milliers d'IV faibles pour cracker la clef WEP

## WPA

* Nouveau contrôle d'intégrité (MIC), protégé cryptographiquement
    * clef de hash dérivée du handshake
* TKIP pour la clef partagée
* Taille de l'IV doublée
* Séquence pseudo-aléatoire jamais réutilisée
* Message : chiffrement / intégrité
    * Pairwise/Unicast : TK / MICK
    * Groupe : GTK / GMIC
    * Envoi de clefs : KEK / KCK
* Confidentialité :
    * clefs dérivées pendant le handshake
    * construites à partir d'une passphrase
    * permet le déchiffrement à condition d'avoir la capture du handshake

## WPA2

* AES utilisé pour chiffrer les données
* Intégrité : MAC utilisant AES

## WPA2 Enterprise

* Authentification : tunnel TLS, serveur RADIUS
* Confidentialité : AES CTR
* Intégrité : MAC AES CBC
* Clef de 256 bits
