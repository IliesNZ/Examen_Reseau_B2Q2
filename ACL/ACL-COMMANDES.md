# MÉMO COMMANDES : LES ACL NUMÉROTÉES EN ENTRÉE (IN)

## 1. CRÉATION DE L'ACL NUMÉROTÉE (Mode configuration globale)

Syntaxe Standard (Numéros de 1 à 99) :
Router(config)# access-list [numero] {deny | permit | remark [texte]} [ip-source] [masque-générique]

Exemple 1 - Autoriser un réseau complet (ex: le LAN 10) :
R1(config)# access-list 10 permit 192.168.10.0 0.0.0.255

Exemple 2 - Bloquer une seule machine précise (avec le mot-clé host) :
R1(config)# access-list 10 deny host 192.168.10.50

Exemple 3 - Autoriser tout le reste (indispensable pour contrer le "deny any" invisible à la fin) :
R1(config)# access-list 10 permit any

---

## 2. APPLICATION EN DIRECTION "IN" (Mode configuration d'interface)

Syntaxe :
Router(config-if)# ip access-group [numero-acl] in

Exemple de mise en place sur le port où le trafic arrive :
R1(config)# interface GigabitEthernet 0/0/0
R1(config-if)# ip access-group 10 in
R1(config-if)# end

---

## 3. VÉRIFICATION

Voir le contenu de l'ACL et le nombre de paquets interceptés (matches) :
R1# show access-lists

Vérifier sur quelle interface et dans quelle direction l'ACL est appliquée :
R1# show ip interface GigabitEthernet 0/0/0 | include access list
(Le résultat affichera : Inbound access list is 10 / Outbound access list is not set)

[⬅️ Vers la théorie](ACL-THEORIE.md)

[⬅️ Vers le README](../README.md)