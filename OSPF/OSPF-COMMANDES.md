# # EXEMPLE DES COMMANDES OSPF - CISCO

## Configuration globale OSPF 

```
router ospf 10
 router-id 1.1.1.1
 ! Déclaration des interfaces via leur IP exacte
 network 10.10.1.1 0.0.0.0 area 0
 network 10.1.1.5 0.0.0.0 area 0
 network 10.1.1.14 0.0.0.0 area 0
 ! Optionnel : Ajuster la bande passante de référence pour le Gigabit (1000 Mbps)
 auto-cost reference-bandwidth 1000
 exit
 ```

## Paramétrage des interfaces
 
```
interface GigabitEthernet 0/0/0
 ! Force R1 à devenir le DR sur ce segment réseau
 ip ospf priority 255
 exit

interface GigabitEthernet 0/0/1
 ! Force le coût OSPF à 30
 ip ospf cost 30
 exit

interface Loopback 0
 ! Force le coût OSPF à 10
 ip ospf cost 10
 exit
 ```
 
 
 SUR R2
 
 ## Configuration de l'interface de sortie 

 ```
interface Loopback 1
 ip address 64.100.0.1 255.255.255.252
 exit
```

## Création de la route par défaut 

```
ip route 0.0.0.0 0.0.0.0 loopback 1
```

## Configuration OSPF et injection de la route 

```
router ospf 10
 ! Injecte la route par défaut dans OSPF pour les autres routeurs
 default-information originate
 exit
 ```

[⬅️ Vers la théorie](OSPF-THEORIE.md)

 [⬅️ Vers le README](../README.md)