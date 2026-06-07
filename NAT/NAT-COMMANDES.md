# MÉMO DES COMMANDES NATTAGE - CISCO

## 1) NAT statique

--> Crée un mappage permanent à une seule adresse

```
R2(config)# ip nat inside source static 192.168.10.254 209.165.201.5       
adresse locale interne --> adresse globale interne

R2(config)# interface serial 0/1/0
R2(config-if)# ip address 192.168.1.2 255.255.255.252
R2(config-if)# ip nat inside
R2(config-if)# exit
R2(config)# interface serial 0/1/1
R2(config-if)# ip address 209.165.200.1 255.255.255.252
R2(config-if)# ip nat outside
```

## 2) NAT dynamique

Utilise un pool d'adresses

```
R2(config)# ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224             entre .226 et .240 = 15 adresses dispos ducoup /27
R2(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R2(config-if)# ip nat inside source list 1 pool NAT-POOL1
R2(config)# Interface serial 0/1/0
R2(config-if)# ip nat inside
R2(config)# interface serial 0/1/1
R2(config-if)# ip nat outside
```

## 3) PAT pour utiliser une seule adresse IPv4

```
R2(config)# ip nat inside source list 1 interface serial 0/1/1 overload
R2(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R2(config)# interface serial 0/1/0
R2(config-if)# ip nat inside
R2(config-if)# exit
R2(config)# interface Serial 0/1/1
R2(config-if)# ip nat outside
```

## 4) PAT pour utiliser un pool d'adresses

```
R2(config)# ip nat pool NAT-POOL2 209.165.200.226 209.165.200.240 netmask 255.255.255.224
R2(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R2(config)# ip nat inside source list 1 pool NAT-POOL2 overload
R2(config)# interface serial 0/1/0
R2(config-if)# ip nat inside
R2(config-if)# exit
R2(config)# interface serial 0/1/1
R2(config-if)# ip nat outside

R2# show ip nat translations
R2# show ip nat translation verbose                pour plus d'info
R2# show ip nat statistics
```

[⬅️ Vers la théorie](NAT-THEORIE.md)

[⬅️ Vers le README](../README.md)