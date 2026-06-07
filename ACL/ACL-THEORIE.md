# MÉMO : LISTES DE CONTRÔLE D'ACCÈS (ACL) - CISCO

## 1. Théorie Fondamentale des ACL

Une ACL est une liste séquentielle de règles (appelées ACE pour Access Control Entry) qui permettent de filtrer le trafic réseau. Le routeur lit l'ACL de haut en bas : dès qu'une règle correspond au paquet, l'action (permit ou deny) est appliquée et le reste de la liste n'est pas lu.

* Le "Deny Any" implicite : C'est la règle d'or. À la fin de chaque ACL, il y a une règle invisible qui bloque tout le reste. Si un paquet ne correspond à aucune des règles explicites de votre liste, il sera détruit.
* Masque générique (Wildcard Mask) : Il fonctionne à l'inverse d'un masque de sous-réseau. 
    * 0 = Le bit de l'adresse IP doit correspondre exactement.
    * 1 = Le bit de l'adresse IP est ignoré.
* Les deux types d'ACL IPv4 :
    * Standards (numérotées de 1 à 99 et 1300 à 1999) : Filtrent uniquement sur l'adresse IP source. Règle de placement : À appliquer le plus près possible de la destination.
    * Étendues (numérotées de 100 à 199 et 2000 à 2699) : Filtrent sur l'IP source, l'IP de destination, le protocole (TCP, UDP...) et les ports. Règle de placement : À appliquer le plus près possible de la source.

---

## 2. Syntaxe et Création des ACL Standards

Les ACL standards autorisent ou refusent les paquets en se basant uniquement sur l'adresse IPv4 source.

Syntaxe de base de création (en mode de configuration globale) :
Router(config)# access-list [access-list-number] {deny | permit | remark [text]}

### Les mots-clés de simplification : "host" et "any"
Pour éviter d'écrire des masques génériques complexes, on utilise des mots-clés :

* host : Remplace le masque générique 0.0.0.0. Tous les bits doivent correspondre pour filtrer une seule adresse d'hôte.
    * Exemple (ces deux commandes sont identiques) :
      R1(config)# access-list 10 permit 192.168.10.10 0.0.0.0
      R1(config)# access-list 10 permit host 192.168.10.10

* any : Remplace le masque générique 255.255.255.255. Ignore l'intégralité de l'adresse IPv4 (accepte n'importe quelle adresse).
    * Exemple (ces deux commandes sont identiques) :
      R1(config)# access-list 11 permit 0.0.0.0 255.255.255.255
      R1(config)# access-list 11 permit any

### Les commentaires (remark)
Ajout de texte descriptif pour la lisibilité de la configuration.

Exemple de création avec commentaires et plusieurs règles :
R1(config)# access-list 10 remark ACE permits ONLY host 192.168.10.10 to the internet
R1(config)# access-list 10 permit host 192.168.10.10
R1(config)# access-list 10 remark ACE permits all host in LAN 2
R1(config)# access-list 10 permit 192.168.20.0 0.0.0.255

---

## 3. Application des ACL

Une ACL configurée ne sert à rien tant qu'elle n'est pas appliquée à une interface réseau ou à une ligne d'administration.

### Application sur une interface physique (Routage)
Le mot-clé "in" filtre le trafic entrant dans le routeur, et "out" filtre le trafic sortant.
S'applique dans le mode de configuration de l'interface.

Syntaxe :
Router(config-if)# ip access-group {access-list-number | access-list-name} {in | out}

Exemple de mise en place en sortie sur une interface série :
R1(config)# interface Serial 0/1/0
R1(config-if)# ip access-group 10 out
R1(config-if)# end

### Sécurisation des lignes VTY (Accès distant via SSH/Telnet)
Utilisation d'une ACL standard pour restreindre les équipements qui ont le droit de prendre la main sur le routeur. On utilise "access-class" au lieu de "ip access-group".
S'applique sur la configuration de la ligne VTY.

Syntaxe :
R1(config-line)# access-class {access-list-number | access-list-name} { in | out }

Exemple de sécurisation de l'accès SSH avec une liste nommée "ADMIN-HOST" :
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# transport input ssh
R1(config-line)# access-class ADMIN-HOST in
R1(config-line)# end

---

## 4. Commandes de vérification

Vérifier le contenu et si du trafic a été intercepté (matches) :

* Voir toutes les ACL configurées avec leurs numéros de séquence :
  R1# show access-lists
  (Ou "R1(config)# do show access-lists" depuis le mode global)

* Filtrer le fichier de configuration pour n'afficher que les ACL :
  R1# show run | section access-list

Aperçu du résultat d'un "show access-lists" :
Standard IP access list 10
    10 permit 192.168.10.10
    20 permit 192.168.20.0, wildcard bits 0.0.0.255