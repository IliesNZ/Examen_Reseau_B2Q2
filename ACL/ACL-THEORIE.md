# MÉMO THÉORIQUE : LES ACL NUMÉROTÉES EN ENTRÉE (IN)

## 1. Le Concept de l'ACL Numérotée

Cisco identifie les listes de contrôle d'accès (ACL) par un numéro. Ce numéro n'est pas choisi au hasard, il indique au routeur le type de filtrage que tu souhaites appliquer :

* Les ACL Standards (Numéros 1 à 99) : 
  Elles sont "basiques". Elles ne regardent qu'une seule chose dans le paquet réseau : l'adresse IP source (d'où vient le paquet).
* Les ACL Étendues (Numéros 100 à 199) : 
  Elles sont "précises". Elles regardent l'IP source, l'IP de destination, et surtout les protocoles (TCP, UDP, ICMP) et les numéros de ports (ex: port 80 pour le web, 22 pour SSH).

## 2. La Direction "IN" (Inbound / Entrant)

L'application d'une ACL sur une interface agit comme un agent de sécurité. La direction "IN" signifie que le routeur inspecte le trafic au moment où il *entre* dans l'interface, avant même de réfléchir à sa destination.

L'ordre des opérations du routeur pour la direction IN :
1. Le paquet arrive sur l'interface (ex: GigabitEthernet 0/0).
2. Le routeur regarde s'il y a une ACL appliquée en "IN".
3. Si OUI, il lit l'ACL de haut en bas :
   - S'il trouve un "permit" (autoriser) : Il laisse passer le paquet et passe à l'étape 4.
   - S'il trouve un "deny" (refuser) : Il détruit le paquet immédiatement (drop).
4. Le routeur consulte sa table de routage pour savoir par où faire ressortir le paquet autorisé.

L'avantage majeur du IN :
C'est la méthode la plus performante. En détruisant les paquets indésirables dès leur arrivée, le routeur n'utilise pas sa puissance de calcul (CPU) pour chercher un itinéraire de routage à un paquet qui finira de toute façon à la poubelle.

## 3. Les Trois Règles d'Or de l'ACL IN

1. Lecture séquentielle : Le routeur lit les règles (ACE) de haut en bas. Dès qu'une condition correspond au paquet, l'action est appliquée et la lecture s'arrête. Il faut donc toujours placer les règles les plus spécifiques en haut, et les plus générales en bas.
2. Le "Deny Any" implicite : C'est le piège classique. À la toute fin de chaque ACL, il existe une règle invisible "deny any" (tout refuser). Si un paquet traverse toute ta liste sans correspondre à aucune de tes règles "permit", il sera automatiquement détruit.
3. Une par interface : On ne peut appliquer qu'UNE SEULE ACL par interface, par protocole routé (IPv4/IPv6) et par direction. (Donc une seule ACL IPv4 en IN par port).

## 4. Le Masque Générique (Wildcard Mask)

Il est utilisé pour indiquer au routeur quelle partie de l'adresse IP il doit comparer avec précision.
* Le bit 0 : Indique que le routeur doit vérifier cette position (ça doit correspondre exactement).
* Le bit 1 : Indique que le routeur s'en moque (il ignore cette position).

* Exemple 1 : 0.0.0.0 (Mot-clé "host") = L'IP complète doit correspondre exactement (une seule machine).
* Exemple 2 : 0.0.0.255 = Les 3 premiers nombres doivent correspondre, le dernier peut être n'importe quoi (un sous-réseau entier en /24).
* Exemple 3 : 255.255.255.255 (Mot-clé "any") = Le routeur ignore tout, l'adresse peut être n'importe laquelle.

[⬅️ Vers les commandes](ACL-COMMANDES.md)

[⬅️ Vers le README](../README.md)