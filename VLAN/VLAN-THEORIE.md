# Synthèse Théorique des VLANs et Trunks

## 1. Concepts de Base des VLANs
* Les VLANs (Réseaux Locaux Virtuels) permettent de segmenter logiquement un réseau physique.
* Chaque VLAN est identifié par un numéro unique (le VLAN ID).
* Pour faciliter l'organisation et la gestion, il est possible d'attribuer un nom descriptif (comme "ADMINISTRATION") à chaque VLAN.

## 2. Les Types de Ports

### Les Ports d'Accès
* Ils sont destinés aux équipements terminaux, tels que les ordinateurs de bureau ou les imprimantes.
* Le trafic sur ces ports ne comporte pas d'étiquette (tag) VLAN.
* Sur ces ports, il est recommandé de désactiver les protocoles de négociation automatique, comme le DTP.

### Les Ports Trunk
* Ils sont utilisés pour transporter le trafic de multiples VLANs simultanément, généralement entre deux commutateurs ou vers un routeur.
* La méthode la plus sécurisée et recommandée en production est de forcer manuellement le mode Trunk, plutôt que de laisser les équipements négocier ce statut.
* **Sécurité des Trunks :** Il est conseillé de désactiver l'envoi de messages de négociation DTP pour éviter des failles de sécurité. De plus, il est préférable de modifier le VLAN natif par défaut vers un autre identifiant  et de filtrer explicitement la liste des VLANs autorisés à transiter sur le lien Trunk.

## 3. Le Protocole DTP (Dynamic Trunking Protocol)
Bien que la configuration manuelle soit préconisée, le DTP est un mécanisme automatique de négociation des liens essentiel à maîtriser sur le plan théorique.
* **Mode Dynamic Auto :** Le port est passif ; il attend qu'une demande de création de Trunk lui parvienne de l'équipement voisin. C'est la configuration par défaut sur de nombreux commutateurs.
* **Mode Dynamic Desirable :** Le port est proactif ; il demande activement à l'équipement voisin de basculer en mode Trunk.
* **Règles de Négociation :** * Si deux ports se font face en mode "Auto", ils resteront de simples ports d'accès.
  * Si un port "Auto" fait face à un port "Desirable", un lien Trunk s'établira avec succès.
  * Si un port est forcé manuellement en mode "Trunk" face à un port en mode "Auto" ou "Desirable", le lien Trunk montera également.

## 4. VLANs à Usages Spécifiques
* **Le VLAN Voix :** Il s'agit d'un réseau dédié au matériel de téléphonie IP, souvent configuré avec des règles de confiance (Trust CoS) pour prioriser la qualité du trafic vocal.
* **Le VLAN de Gestion (SVI - Switched Virtual Interface) :** C'est une interface virtuelle créée pour administrer l'équipement à distance (via ping ou SSH). En pratique, on utilise souvent un VLAN dédié spécifiquement à la gestion, pour ne pas utiliser le VLAN 1 par défaut. Si le commutateur doit pouvoir répondre à des requêtes provenant d'un autre sous-réseau distant, il est impératif de lui configurer une passerelle par défaut.

## 5. Sécurité et Bonnes Pratiques d'Isolation
* Une règle d'or en sécurité réseau est de ne jamais laisser de ports inutilisés ouverts sur le réseau de production.
* La meilleure pratique consiste à créer un VLAN dédié (souvent surnommé VLAN "Parking" ou "Black Hole"), d'y assigner toutes les interfaces non utilisées, puis de les désactiver physiquement.

## 6. Diagnostic et Remise à Zéro
* **Réinitialisation :** Pour remettre un commutateur à zéro concernant ses VLANs, il faut impérativement procéder à un reset complet  qui inclut la suppression de la base de données locale des VLANs.
* **Vérifications :** Différents niveaux de diagnostic permettent d'afficher un résumé global des VLANs , de cibler les informations d'un VLAN spécifique via son numéro d'identification  ou son nom , de lister l'état des liens Trunks , et de contrôler le statut exact des négociations du protocole DTP.