# MÉMO THÉORIQUE : LE NAT (Network Address Translation)

## 1. Pourquoi utiliser le NAT ?

* Le problème : Le protocole IPv4 ne dispose pas d'assez d'adresses publiques pour connecter tous les équipements de la planète à Internet.
* La solution : L'espace d'adressage a été divisé en deux :
    * Adresses privées (RFC 1918) : Gratuites, utilisées dans les réseaux locaux (LAN), mais non routables sur Internet (ex: 192.168.x.x, 10.x.x.x).
    * Adresses publiques : Payantes, uniques au monde, et routables sur Internet.
* Le rôle du NAT : Placé sur le routeur frontière, le NAT traduit les adresses IP privées en adresses IP publiques (pour le trafic sortant) et inversement (pour le trafic entrant).

## 2. Le Vocabulaire Cisco (Indispensable)

Cisco utilise 4 termes précis pour désigner l'état d'une adresse lors de la traduction :
* Inside Local (Locale interne) : L'adresse IP privée de ton équipement dans le réseau local (ex: 192.168.10.254).
* Inside Global (Globale interne) : L'adresse IP publique par laquelle ton équipement est vu sur Internet après la traduction du routeur.
* Outside Global (Globale externe) : L'adresse IP publique de la destination sur Internet (ex: le serveur web que tu contactes).
* Outside Local (Locale externe) : L'adresse IP de la destination telle qu'elle est connue par les hôtes de ton réseau interne (très souvent identique à l'Outside Global).

## 3. Les Trois Types de Translation

A) Le NAT Statique (Static NAT)
* Principe : Mappage permanent 1-pour-1 entre une IP locale interne et une IP globale interne. 
* Cas d'usage : Hébergement. C'est indispensable lorsqu'un appareil interne (comme un serveur Web ou un serveur mail) doit être accessible depuis Internet avec une adresse publique fixe.

B) Le NAT Dynamique (Dynamic NAT)
* Principe : Mappage Plusieurs-vers-Plusieurs. Le routeur possède un "pool" (groupe) d'adresses IP publiques. Quand un hôte interne veut sortir, le routeur lui attribue temporairement une IP publique disponible dans ce pool.
* Limite : Si le pool contient 15 adresses publiques, seuls 15 appareils pourront aller sur Internet en même temps. Le 16ème sera bloqué jusqu'à ce qu'une session se termine. C'est peu utilisé pour le simple accès Internet.

C) Le PAT (Port Address Translation) / NAT Overload
* Principe : Mappage Plusieurs-vers-Un. Permet à tout un réseau local de sortir sur Internet en utilisant une seule adresse IP publique (ou un petit pool d'adresses).
* Comment ça marche ? Le routeur utilise les numéros de port source (couche 4) pour différencier les flux de chaque PC interne. 
    * Exemple : Le PC1 sort avec l'IP publique 209.165.200.1:5001. Le PC2 sort avec la même IP 209.165.200.1:5002.
* Cas d'usage : C'est le mode le plus utilisé au monde (c'est ce que fait une box Internet classique pour que tous les appareils de la maison naviguent avec la même IP publique).

## 4. La Table NAT
Pour que les réponses d'Internet reviennent à la bonne machine, le routeur enregistre chaque traduction active dans sa table de routage NAT. Quand un paquet de réponse arrive, il consulte cette table, traduit l'adresse IP publique de destination en l'adresse IP privée d'origine, et transmet le paquet sur le LAN.

[⬅️ Vers les commandes](NAT-COMMANDES.md)

[⬅️ Vers le README](../README.md)