# Examen_Réseau_B2Q2

## VLAN/TRUNK

### - [Théorie](VLAN/VLAN-THEORIE.md)

### - [Commandes](VLAN/VLAN-COMMANDES.md)

## ROUTAGE

### - [Commandes](ROUTAGE/ROUTAGE-COMMANDES.md)

## NAT

### - [Théorie](NAT/NAT-THEORIE.md)

### - [Commandes](NAT/NAT-COMMANDES.md)

## ACL

### - [Théorie](ACL/ACL-THEORIE.md)

### - [Commandes](ACL/ACL-COMMANDES.md)

## OSPF - Il n'y a pas à l'examen :)

### - [Théorie](OSPF/OSPF-THEORIE.md)

### - [Commandes](OSPF/OSPF-COMMANDES.md)

# Examen blanc

VTY :

```
Switch0(config)# interface vlan 1
Switch0(config-if)# ip address 192.168.X.X 255.255.255.0  (L'IP que tu as prévue pour ton switch)
Switch0(config-if)# no shutdown
Switch0(config-if)# exit
Switch0(config)# ip default-gateway 192.168.X.254 (L'IP de ton Router0 pour ce VLAN)

Router(config)# hostname Router0
Router0(config)# ip domain-name iliesnz.com
Router0(config)# crypto key generate rsa

Router0(config)# username admin privilege 15 secret TonMotDePasse

Router0(config)# line vty 0 4
Router0(config-line)# login local
Router0(config-line)# transport input ssh
Router0(config-line)# exit
```

VLAN :

```
Switch0(config)# vlan 10
Switch0(config-vlan)# name ADMIN
Switch0(config-vlan)# vlan 20
Switch0(config-vlan)# name USERS
Switch0(config-vlan)# vlan 30
Switch0(config-vlan)# name GUEST
Switch0(config-vlan)# vlan 40
Switch0(config-vlan)# name SERVEURS
Switch0(config-vlan)# exit

! Pour le PC ADMIN
Switch0(config)# interface fa0/1
Switch0(config-if)# switchport mode access
Switch0(config-if)# switchport access vlan 10

! Pour le PC USERS
Switch0(config)# interface fa0/2
Switch0(config-if)# switchport mode access
Switch0(config-if)# switchport access vlan 20

! Et tu répètes l'opération pour Fa0/4 (vlan 30) et Fa0/5 (vlan 40)

Switch0(config)# interface gig0/0
Switch0(config-if)# switchport mode trunk
Switch0(config-if)# switchport nonegotiate
Switch0(config-if)# switchport trunk native vlan 99
Switch0(config-if)# switchport trunk allowed vlan 10,20,30,40,99
Switch0(config-if)# exit

! Sous-interface pour le VLAN 10 (ADMIN)
Router0(config)# interface gig0/0.10
Router0(config-subif)# encapsulation dot1Q 10
Router0(config-subif)# ip address 192.168.10.254 255.255.255.0
Router0(config-subif)# exit
```