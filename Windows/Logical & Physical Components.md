Voici une présentation des **composants physiques** et **composants logiques** dans le cadre d'Active Directory (AD DS), avec leur description :

### **Composants Physiques**

| **Composant Physique**                             | **Description**                                                                                                                                                  |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Contrôleurs de domaine**                         | Contiennent des copies de la base de données AD DS.                                                                                                              |
| **Magasin de données**                             | Fichier sur chaque contrôleur de domaine qui stocke les informations AD DS.                                                                                      |
| **Serveurs de catalogue global**                   | Hébergent le catalogue global, une copie partielle, en lecture seule, de tous les objets dans la forêt. Ce catalogue permet d'accélérer les recherches d'objets. |
| **Contrôleurs de domaine en lecture seule (RODC)** | Installation spéciale d'AD DS en lecture seule, souvent utilisée dans les filiales où la sécurité et l'assistance informatique sont moins avancées.              |

### **Composants Logiques**

| **Composant Logique**         | **Description**                                                                                                                                                        |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Partition**                 | Une section de la base de données AD DS. Bien que la base de données soit un seul fichier (NTDS.DIT), elle est gérée et répliquée sous forme de partitions distinctes. |
| **Schéma**                    | Définit la liste des types d'objets et d'attributs que tous les objets dans AD DS peuvent avoir.                                                                       |
| **Domaine**                   | Limite d'administration logique pour les utilisateurs et les ordinateurs.                                                                                              |
| **Arborescence de domaine**   | Collection de domaines qui partagent un domaine racine commun et un espace de noms DNS.                                                                                |
| **Forêt**                     | Collection de domaines qui partagent un service AD DS commun.                                                                                                          |
| **Site**                      | Collection d'utilisateurs, groupes et ordinateurs définis par leurs emplacements physiques. Les sites sont utilisés pour la gestion de la réplication AD DS.           |
| **Unité d'organisation (OU)** | Conteneur dans AD DS permettant de déléguer des droits d'administration et de lier des objets de stratégie de groupe (GPO).                                            |

Ces composants permettent d'organiser et de gérer efficacement les objets dans un environnement Active Directory, tant au niveau physique que logique.

La **structure logique d'Active Directory (AD)** est organisée en plusieurs éléments qui permettent de gérer efficacement les ressources et les utilisateurs au sein d'un réseau. Voici les principales composantes de cette structure :

### **1. Schéma (Schema)**

- **Description** : Le schéma d'Active Directory définit les types d'objets et les attributs que ces objets peuvent avoir. Il constitue la base de données de toutes les informations relatives aux objets du domaine.
- **Exemple** : Un objet utilisateur dans Active Directory pourrait avoir des attributs tels que le nom, l'adresse email, le mot de passe, etc.

### **2. Forêt (Forest)**

- **Description** : La forêt est l'unité la plus large d'Active Directory. Une forêt est composée de plusieurs domaines qui partagent un schéma et une configuration AD DS communs. Les forêts sont indépendantes les unes des autres et peuvent être reliées via des relations de confiance.
- **Exemple** : Une entreprise ayant plusieurs filiales dans différents pays peut créer une forêt unique pour gérer tous ses domaines de manière centralisée.

### **3. Domaine (Domain)**

- **Description** : Un domaine est une unité logique d'administration dans Active Directory. Il représente une zone de gestion qui regroupe des utilisateurs, des ordinateurs, des groupes, etc. Chaque domaine possède sa propre base de données Active Directory (NTDS).
- **Exemple** : "corp.monsite.com" pourrait être un domaine représentant l'ensemble des ressources d'une organisation.

### **4. Arborescence de domaine (Domain Tree)**

- **Description** : Une arborescence de domaine est un groupe de domaines qui partagent un nom de domaine racine commun et un espace de noms DNS. Chaque domaine dans l'arborescence peut avoir ses propres sous-domaines.
- **Exemple** : "corp.monsite.com" et "support.monsite.com" sont deux domaines différents qui partagent un espace de noms DNS commun.

### **5. Unité d’organisation (OU - Organizational Unit)**

- **Description** : Une unité d'organisation est un conteneur dans Active Directory qui permet de regrouper des objets tels que des utilisateurs, des groupes, des ordinateurs, etc. Les OUs permettent d'appliquer des stratégies de groupe (GPO) et de déléguer des permissions d'administration.
- **Exemple** : Une OU pourrait être nommée "Ressources humaines" pour contenir tous les utilisateurs et ordinateurs de ce département.

### **6. Objet (Object)**

- **Description** : Un objet dans Active Directory est une entité qui peut être un utilisateur, un groupe, un ordinateur, une imprimante, ou tout autre type de ressource réseau. Chaque objet possède un ensemble d'attributs définis par le schéma.
- **Exemple** : Un utilisateur est un objet dans Active Directory qui possède des attributs comme le nom, l'adresse email, et d'autres informations liées à l'utilisateur.

### **7. Partition**

- **Description** : Une partition est une section logique de la base de données AD DS. Elle permet de diviser les données pour en faciliter la gestion et la réplication. Les partitions sont répliquées sur les contrôleurs de domaine en fonction des besoins.
- **Exemple** : La partition de domaine contient des informations sur les objets du domaine, tandis que la partition du schéma contient des informations sur les types d'objets.

### **8. Catalogue Global (Global Catalog)**

- **Description** : Le catalogue global est une base de données partielle et en lecture seule qui contient des informations sur tous les objets dans une forêt Active Directory. Il est utilisé pour accélérer les recherches dans Active Directory, notamment pour des objets situés dans des domaines différents.
- **Exemple** : Lorsque vous effectuez une recherche dans AD DS, le catalogue global permet de trouver rapidement des objets provenant de différents domaines de la forêt.

### **9. Relation de Confiance (Trust Relationship)**

- **Description** : Une relation de confiance est une association entre deux domaines qui permet aux utilisateurs d'un domaine d'accéder aux ressources d'un autre domaine. Ces relations peuvent être unidirectionnelles ou bidirectionnelles.
- **Exemple** : Un domaine "corp.monsite.com" peut établir une relation de confiance avec un domaine partenaire "partenaire.monsite.com" pour permettre aux utilisateurs des deux domaines d'accéder aux ressources de l'autre.

### **Conclusion**

La structure logique d'Active Directory est conçue pour organiser et centraliser la gestion des ressources et des utilisateurs dans un réseau. Chaque composant joue un rôle spécifique, permettant de garantir une gestion sécurisée, flexible et évolutive des ressources d'une organisation.