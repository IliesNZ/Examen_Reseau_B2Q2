
# MÉMO DES COMMANDES OSPF - CISCO

## 1. ACTIVATION ET CONFIGURATION GLOBALE

<b>router ospf [process-id] :</b> Active le processus OSPF (ex: router ospf 10).


<b>router-id [A.B.C.D] :</b> Attribue un identifiant IP unique au routeur pour OSPF.

<b>network [IP] [masque-générique] area [zone-id] :</b> Déclare un réseau ou une IP spécifique à inclure dans OSPF (ex: network 10.1.1.1 0.0.0.0 area 0).

<b>auto-cost reference-bandwidth [Mbps] :</b> Ajuste le calcul du coût pour les liens très rapides (ex: 1000 pour du Gigabit).

<b>default-information originate :</b> Diffuse la route par défaut configurée sur ce routeur à tous les autres voisins OSPF.


## 2. PARAMÉTRAGE DIRECT SUR LES INTERFACES

<b>interface [type] [numéro] :</b> Entre dans la configuration d'une interface (ex: interface g0/0/0 ou loopback 0).

<b>ip ospf [process-id] area [zone-id] :</b> Active OSPF directement sur l'interface (méthode moderne, alternative à la commande 'network').

<b>ip ospf priority [0-255] :</b> Modifie la priorité pour l'élection du chef de réseau (DR). 255 = priorité max, 0 = ne participe pas.

<b>ip ospf cost [valeur] :</b> Force manuellement le coût (la métrique) OSPF de l'interface pour influencer le choix du chemin.

<b>ip ospf hello-interval [secondes] :</b> Modifie l'intervalle d'envoi des messages de "coucou" (Hello) entre voisins.

<b>ip ospf dead-interval [secondes] :</b> Modifie le temps d'attente maximum avant de déclarer qu'un voisin OSPF est hors ligne.


## 3. COMMANDES SYSTÈMES ET ROUTAGE STATIQUE 

<b>ip address [IP] [masque-sous-réseau] :</b> Assigne une adresse IP classique à l'interface (ex: ip address 64.100.0.1 255.255.255.252).

<b>ip route 0.0.0.0 0.0.0.0 [interface-de-sortie ou IP-suivante] :</b> Crée une route par défaut statique (vers Internet, par exemple).


## 4. VÉRIFICATION ET DÉPANNAGE (En mode privilégié #)

<b>show ip ospf neighbor :</b> Affiche les voisins OSPF détectés et leur état de connexion (ex: Full, 2-Way).

<b>show ip route ospf :</b> Affiche la table de routage, en filtrant uniquement les routes apprises grâce à OSPF.

<b>show ip ospf interface [type] [numéro] :</b> Affiche les détails OSPF d'une interface précise (son coût, si elle est DR/BDR, ses timers).

<b>clear ip ospf process :</b> Redémarre le processus OSPF. Obligatoire pour appliquer un nouveau router-id ou une nouvelle priorité.

[⬅️ Vers les commandes](OSPF-COMMANDES.md)

[⬅️ Vers le README](../README.md)