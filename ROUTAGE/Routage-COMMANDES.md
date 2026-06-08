# MÉMO COMMANDES : LE ROUTAGE

## 1) Routage statique

```
! ip route [Réseau de destination] [Masque] [IP du prochain saut]
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

## 2) Routage dynamique

```
router ospf 1
 ! Annoncer le réseau 192.168.10.0/24 dans l'aire 0
 network 192.168.10.0 0.0.0.255 area 0
 exit
 ```
  
 [⬅️ Vers le README](../README.md)