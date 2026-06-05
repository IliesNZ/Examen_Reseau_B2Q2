# MEMO VLAN


## 1. CRÉATION DES VLAN 
(Mode configuration globale)

```
Switch(config)# vlan 10                     	 ! Créer le VLAN ID 10
Switch(config-vlan)# name ADMINISTRATION   		 ! Nommer le VLAN
Switch(config-vlan)# exit
```

## 2. CONFIGURATION DES PORTS D'ACCÈS 
(Pour les PC, Imprimantes - Pas de tag)

```
Switch(config)# interface fa0/6
Switch(config-if)# switchport mode access   		! Désactive la négo Trunk (DTP)
Switch(config-if)# switchport access vlan 10
Switch(config-if)# no shutdown
```

## 3. CONFIGURATION DES PORTS TRUNK (MANUEL) 
(Méthode recommandée : on force le Trunk)

```
Switch(config)# interface gi0/1
Switch(config-if)# switchport mode trunk           			   	   ! Force le Trunk
Switch(config-if)# switchport nonegotiate         				   ! (Sécurité) Désactive les messages DTP
Switch(config-if)# switchport trunk native vlan 99   			   ! Change le VLAN natif
Switch(config-if)# switchport trunk allowed vlan 10,20,99 		   ! Filtre les VLANs
```

## 4. DTP (DYNAMIC TRUNKING PROTOCOL) 
(Méthode automatique - À connaître pour l'examen théorique)

```
Switch(config-if)# switchport mode dynamic auto      	! Attend que l'autre côté demande le Trunk (défaut sur bcp de switchs)
Switch(config-if)# switchport mode dynamic desirable 	! Demande activement à l'autre côté de passer en Trunk

! Tableau récapitulatif DTP :
! Auto + Auto = Access
! Auto + Desirable = Trunk
! Desirable + Desirable = Trunk
! Trunk (force) + Auto/Desirable = Trunk
```

## 5. VLAN VOIX (TÉLÉPHONIE) 

```
Switch(config-if)# switchport voice vlan 150
Switch(config-if)# mls qos trust cos
```

## 6. VLAN DE GESTION (SVI) 
(Pour pouvoir pinguer le switch ou prendre la main en SSH)

```
Switch(config)# interface vlan 99           				! On utilise souvent un VLAN dédié gestion (pas le 1)
Switch(config-if)# ip address 192.168.99.2 255.255.255.0
Switch(config-if)# no shutdown
! Si le switch doit sortir du réseau local (répondre à un ping distant) :
Switch(config)# ip default-gateway 192.168.99.1
```

## 7. SÉCURITÉ (BONNES PRATIQUES) 
(Mettre les ports non utilisés dans un VLAN "Parking" ou "Black Hole")

```
Switch(config)# vlan 999
Switch(config-vlan)# name PARKING
Switch(config)# interface range fa0/10 - 24
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 999
Switch(config-if-range)# shutdown
```

## 8. SUPPRESSION & DIAGNOSTIC 

```
! Reset complet VLAN
Switch# delete flash:vlan.dat
Switch# erase startup-config
Switch# reload

! Vérifications
Switch# show vlan brief             ! Résumé
Switch# show vlan id 10             ! Info sur un VLAN précis
Switch# show vlan name PROFS        ! Info par nom
Switch# show interfaces trunk       ! Info Trunks
Switch# show dtp interface gi0/1    ! Voir l'état de la négociation DTP
Switch# show interfaces fa0/1 switchport ! Détails complets couche 2
```