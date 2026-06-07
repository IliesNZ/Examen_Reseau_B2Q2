# MÉMO COMMANDES : LISTES DE CONTRÔLE D'ACCÈS (ACL) - CISCO

## 1. CRÉATION DES ACL STANDARDS (Mode de configuration globale)

Syntaxe de base :

```
Router(config)# access-list [access-list-number] {deny | permit | remark [text]}

Exemple classique avec masque générique (wildcard) :
R1(config)# access-list 10 permit 192.168.20.0 0.0.0.255

Utilisation des mots-clés "host" (remplace 0.0.0.0) et "any" (remplace 255.255.255.255) :
R1(config)# access-list 10 permit host 192.168.10.10
R1(config)# access-list 11 permit any

Ajout de commentaires (remark) pour organiser la liste :
R1(config)# access-list 10 remark ACE permits ONLY host 192.168.10.10 to the internet
R1(config)# access-list 10 permit host 192.168.10.10
R1(config)# access-list 10 remark ACE permits all host in LAN 2
R1(config)# access-list 10 permit 192.168.20.0 0.0.0.255
```

## 2. APPLICATION DES ACL

Sur une interface physique (Mode configuration d'interface) :
```
Syntaxe : Router(config-if)# ip access-group {access-list-number | access-list-name} {in | out}

Exemple :
R1(config)# interface Serial 0/1/0
R1(config-if)# ip access-group 10 out
R1(config-if)# end

Sur les lignes VTY / Accès distant (Mode configuration de ligne) :
Syntaxe : R1(config-line)# access-class {access-list-number | access-list-name} { in | out }

Exemple :
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# transport input ssh
R1(config-line)# access-class ADMIN-HOST in
R1(config-line)# end
```

## 3. VÉRIFICATION (Mode d'exécution privilégié)

Afficher toutes les ACL configurées avec leurs numéros de séquence :
```
R1# show access-lists
(Ou "R1(config)# do show access-lists" depuis le mode configuration globale)

Filtrer le fichier de configuration en cours pour n'afficher que les lignes ACL :
R1# show run | section access-list
```