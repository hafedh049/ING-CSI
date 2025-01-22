> [!tip]
> • VPC = Virtual Private Cloud
> • Création un réseau isolé dans le cloud AWS
> • Contrôle total sur la configuration de l’adresse IP, les sous-réseaux, les tables de routage, et les passerelles réseau

> [!warning]
> **Caractéristiques clés d’un VPC** : 
> • **Isolation des réseaux** : Chaque VPC est isolé des autres, assurant une sécurité renforcée. 
> • **Configuration du réseau** : Contrôle sur les adresses IP, sous-réseaux, routage et connectivité. 
> • **Sécurité renforcée** : Contrôle précis du trafic via des groupes de sécurité et des listes de contrôle d’accès réseau (ACL).

> [!success]
>  • Tous les nouveaux comptes AWS ont un VPC par défaut.
>  • Les nouvelles instances EC2 sont lancées dans le VPC par défaut si aucun sous-réseau n'est spécifié.
>  • Le VPC par défaut dispose d'une connectivité Internet et toutes les instances EC2 qui s'y trouvent ont des adresses IPv4 publiques.
>  • Dans une région AWS, nous pouvons avoir plusieurs VPC (max. 5 par région).

> [!bug]
> Le VPC est privé, seules les plages IPv4 privées sont autorisées : 
> 	**• 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)** 
> 	**• 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)** 
> 	**• 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)**

> [!tip]
> • **CIDR : Classless Inter-Domain Routing)** : Plage d’adresses IP privées définie pour le VPC (exemple : 10.0.0.0/16). Cette plage peut être subdivisée en sous-réseaux. 
> • **Subnet (Sous-réseaux)** : Un VPC peut être divisé en sous-réseaux (**publics ou privés**). Les sous-réseaux publics sont accessibles depuis l’extérieur via Internet, tandis que les sous-réseaux privés ne le sont pas. 
> AWS réserve 5 adresses IP (les 4 premières et la dernière) dans chaque sous-réseau. 
> Exemple : si le bloc CIDR est 10.0.0.0/24, les adresses IP réservées sont les suivantes : 
> **- 10.0.0.0: Adresse réseau** 
> **- 10.0.0.1: réservé par AWS pour le routeur VPC** 
> **- 10.0.0.2: réservé par AWS pour le mappage au DNS fourni par Amazon** 
> **- 10.0.0.3: réservé par AWS pour une utilisation future** 
> **- 10.0.0.255: Adresse de diffusion réseau.**
> • **Routing table** : Définie les routes pour le trafic réseau entre les sous-réseaux et les ressources externes. 
> • **Internet Gateway and NAT Gateway** : Permettent l’accès à Internet aux ressources dans les sous-réseaux publics et privés. 

> [!warning]
>  • Un VPC ne peut être rattaché qu'à un Internet GW et vice versa

# Sous-réseaux publics : 

• Contiennent des ressources accessibles depuis Internet (par exemple, les serveurs web). • Reliés à une Passerelle Internet pour permettre l'accès depuis et vers Internet. 
• Utilisent des groupes de sécurité pour autoriser le trafic entrant et sortant. 

# Sous-réseaux privés : 
• Contiennent des ressources nécessitant un accès interne uniquement (par exemple, des bases de données). 
• Utilisent une NAT Gateway pour permettre aux ressources d'accéder à Internet pour des mises à jour ou des téléchargements de paquets, mais bloquent le trafic entrant non sollicité.

# NAT Instance

> [! note]
> • Une AMI Amazon Linux pré-configurée est disponible.

### Gestion des Groupes de Sécurité et des Règles :
**Entrant:** 
	- Autoriser le trafic HTTP / HTTPS provenant des sous-réseaux privés. 
	- Autoriser l'accès SSH depuis votre réseau domestique (accès fourni via une passerelle Internet). 
**Sortant:** Autoriser le trafic HTTP / HTTPS vers Internet.

# NAT Gateway

> [!tip]
> • Un service géré par AWS qui permet aux instances dans des sous-réseaux privés d’accéder à Internet sans être accessibles depuis l'extérieur.
> • Crée dans une zone de disponibilité spécifique (Availability Zone) et utilise une IP élastique (Elastic IP).
> • Ne peut pas être utilisée par une instance EC2 dans le même sous-réseau (seulement pour d'autres sous-réseaux)
> • Nécessite une passerelle Internet (IGW) pour le routage (Sous-réseau privé => NAT Gateway => IGW).
> • Offre 5 Gbps de bande passante avec une mise à l'échelle automatique jusqu'à 45 Gbps.
> • Aucun groupe de sécurité à gérer ou requis.

# NACL: Network Access List

> [!error]
> • Une fonctionnalité de sécurité dans AWS qui permet de contrôler le trafic entrant et sortant au niveau du sous-réseau dans un Virtual Private Cloud (VPC).
> • Une NACL est assignée par sous-réseau. Les nouveaux sous-réseaux reçoivent la NACL par défaut
> • Les règles NACL sont définies avec un numéro (de 1 à 32766). Un numéro inférieur a une priorité plus élevée.
> • La dernière règle est un astérisque (\*) qui refuse toute demande en cas de non-correspondance avec les règles précédentes.

> [!note]
> • Les NACL nouvellement créées refusent tout le trafic par défaut.

![[Pasted image 20250121155020.png]]

# Ephemeral Ports

> [!important]
> • Les ports éphémères sont des ports temporaires utilisés pour établir des connexions réseau, généralement pour des communications sortantes
> • Sur la plupart des systèmes, y compris AWS, les ports éphémères sont souvent attribués dans une plage définie, généralement entre **1024** et **65535**.

# ENI: Elastic Network Interface

> [!warning]
> • Une interface réseau virtuelle dans AWS qui peut être attachée à une instance EC2 dans un Virtual Private Cloud (VPC). 
> - Caractéristiques de l’ENI: 
> 	 • Adresse IP : Chaque ENI a une adresse IP privée principale et peut avoir des adresses IP secondaires. Elle peut aussi se voir attribuer une adresse IP publique si nécessaire. 
> 	 • Adresse MAC : Une adresse MAC unique reste attachée à l’ENI, même si elle est déplacée entre instances. 
> 	 • Groupes de Sécurité : Des règles de pare-feu peuvent être appliquées pour contrôler le trafic entrant et sortant. 
> 	 • Attachement et Détachement : Les ENI peuvent être attachées/détachées des instances EC2, facilitant le basculement et la redondance en transférant l’interface d’une instance à une autre dans le même VPC.

