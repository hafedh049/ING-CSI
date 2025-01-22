> [!tip] Definition
> **Amazon Web Services (AWS)** est la plateforme cloud la plus complète et adoptée au monde, offrant plus de 200 services pour aider les entreprises à réduire les coûts, améliorer leur agilité et innover rapidement.

> [!note] Cas D'utilisation
> 1. **Hébergement d'applications Web et d’API**
> 2. **Sauvegarde, Archivage et Récupération des Données**
> 3. **Big Data et Analytique**
> 4. **Gaming**

> [!important]
> **AWS Global Infrastructure**
> • AWS Regions 
> • AWS Availability Zones 
> • AWS Local Zones 
> • AWS Edge Locations /Points of Presence

# AWS Regions 

• Une **région** est une **zone géographique**. 
• Chaque région est composée de $2 - 6$ **AZ**. 
• Chaque région Amazon est conçue pour être complètement isolée des autres régions Amazon. 
• Chaque région AWS possède plusieurs AZ et Datacenters.

# AWS Availability Zones 

• Les zones de disponibilité sont physiquement séparées et isolées les unes des autres. 
• Les zones de disponibilité s'étendent sur un ou plusieurs centres de données et disposent de connexions réseau directes, à faible latence, à haut débit et redondantes entre elles. 
• Chaque zone de disponibilité est conçue comme une zone de défaillance indépendante.

# AWS Local Zones

• Une forme de déploiement d'infrastructure AWS qui permet d'exécuter des ressources AWS plus près des utilisateurs finaux, dans des zones métropolitaines éloignées des régions principales d'AWS. 
• **Services fournis** : Elles proposent un sous-ensemble de services AWS, tels que le calcul, le stockage, les bases de données et les services réseau. 
• **Utilisation** : Conçues pour fonctionner avec les régions AWS principales, elles permettent d'étendre l'infrastructure AWS existante aux Local Zones tout en utilisant les mêmes outils, API et services AWS.

# AWS Edge Locations 

• Ce sont des installations déployées dans des emplacements stratégiques dans le monde entier pour fournir des services de mise en cache et de distribution de contenu. 
• Utilisées pour améliorer la distribution du contenu en mettant en cache celui-ci dans des emplacements géographiques distribués, réduisant ainsi la latence des demandes de contenu. 
• Elles sont principalement utilisées par le service CDN (réseau de distribution de contenu) Amazon CloudFront. Lorsqu'un utilisateur demande du contenu d'un site web ou d'une application utilisant CloudFront, la requête est redirigée vers l'Edge Location la plus proche.